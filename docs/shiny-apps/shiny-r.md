
# Shiny R Apps

## What are they made of?
A Shiny app is built in two main parts:

1. UI (User Interface)
- Defines how the app looks — layout, inputs (buttons, sliders, file uploads), outputs (plots, tables, text).
- You specify what the user sees and interacts with.
- Example elements: `actionButton()`, `sliderInput()`, `plotOutput()`, `tableOutput()`.

2. Server
- Contains the logic and calculations behind the app.
- Defines how the app responds to user inputs.
- Creates reactive outputs using `render*()` functions (e.g., `renderPlot()`, `renderTable()`).
- Uses reactive expressions to update outputs automatically when inputs change.

While it's possible to include both the UI and server components in a single app.R script, I recommend separating the UI, server, and any additional analysis code into distinct scripts. This is the structure I will follow throughout the rest of this tutorial.

## How do UI and Server communicate?
- Each UI output element (like `plotOutput("plot1")`), has an ID (i.e., `plot1`) and corresponds to a server-side render function (`output$plot1 <- renderPlot({...})`).
- `output$plot1 <- renderPlot({...})` means "Draw a plot and display it in the UI where the placeholder called `plotOutput("plot1")` is".
- Inputs from the UI (like `input$slider1`) are available in the server code to control input behavior or data processing.
- The reactive programming model ensures the server updates outputs automatically when relevant inputs change.

## Some commonly used libraries:
- shiny – Core framework for building interactive web apps in R. Handles reactivity between UI inputs and server logic.
- shinydashboard – Provides a clean, structured layout system for dashboards using components like headers, sidebars, and value boxes.
- ggplot2 – Powerful plotting library based on the grammar of graphics. Used to create customizable plots like scatterplots, bar charts, and histograms.
- dplyr – Data manipulation toolkit with intuitive functions (`filter()`, `mutate()`, `summarise()`) for transforming and summarising datasets efficiently.
- DT – Enables interactive tables (search, sort, paginate) in Shiny apps.

# Some commonly used functions

## Overview

This guide covers the most commonly used functionalities in Shiny R applications. Shiny follows a reactive programming model where the **UI (User Interface)** defines what users see, and the **server** function defines how the app responds to user interactions.

## Basic Structure

UI and server are the two main components:

```r
library(shiny)

ui <- fluidPage(
  # UI elements go here
)

server <- function(input, output, session) {
  # Server logic goes here
}

shinyApp(ui = ui, server = server)
```

## Essential Server Functions

### The `req()` Function

Use `req()` to ensure code only runs when required inputs exist:

```r
# Don't run unless rawData exists and is not NULL
req(rawData())

# Don't run unless multiple conditions are met
req(input$file, input$column, rawData())
```

### Reactive Values and Functions

```r
# Store data that can change over time
rawData <- reactiveVal(NULL)
processedData <- reactiveVal(NULL)

# Create reactive expressions (computed values that update automatically)
filteredData <- reactive({
  req(rawData())
  # Your filtering logic here
  filter(rawData(), column > input$threshold)
})
```

### Observer Functions

```r
# observeEvent: Responds to specific input changes
observeEvent(input$button, {
  # Code runs when button is clicked
})
```

## File Upload and Data Loading

### UI Components

```r
# Choosing file to uploda
fileInput("upload", 
          "Choose CSV File", 
          accept = c(".csv", ".txt"),
          multiple = FALSE),
# Action button to upload the data 
actionButton("loadBtn", "Load Data")
```

### Server Logic

```r
observeEvent(input$loadBtn, {
  req(input$upload)
  
  # Show loading message
  showNotification("Loading data...", type = "message")
  
  tryCatch({
    # Read the uploaded file
    data <- read.csv(input$upload$datapath, 
                     header = TRUE, 
                     stringsAsFactors = FALSE)
    
    # Store in reactive value
    rawData(data)
    
    # Success message
    showNotification("Data loaded successfully!", type = "message")
    
  }, error = function(e) {
    showNotification(paste("Error loading file:", e$message), type = "error")
  })
})
```

## User Interface Elements

### Buttons

```r
# UI
actionButton("analyzeBtn", "Analyze Data")

# Server
observeEvent(input$analyzeBtn, {
  req(rawData())
  
  # Your analysis code here
  data <- rawData()
  
  # Example analysis
  analyzed_data <- data %>%
    mutate(mean_age = mean(Age, na.rm = TRUE),
           age_category = ifelse(Age > mean_age, "Above Average", "Below Average"))
  
  processedData(analyzed_data)
})
```

### Numeric Inputs and Sliders

```r
# UI
numericInput("threshold", "Set threshold:", 
             value = 50, min = 0, max = 100, step = 1),
sliderInput("range", "Select range:", 
            min = 0, max = 100, value = c(25, 75)),
sliderInput("bins", "Number of bins:", 
            min = 10, max = 50, value = 30)
```

### Selection Inputs

```r
# UI
# Dropdown
selectInput("column", "Choose column:", 
            choices = NULL,  # Will be updated from server
            selected = NULL),
#  Radio buttons
radioButtons("plot_type", "Plot type:",
             choices = list("Histogram" = "hist",
                           "Boxplot" = "box",
                           "Scatter" = "scatter"),
             selected = "hist"),
# Multiple checkboxes
checkboxGroupInput("variables", "Select variables:",
                   choices = NULL)  # Updated from server

# Server - Update choices dynamically
observe({
  req(rawData())
  
  # Update column choices based on loaded data
  updateSelectInput(session, "column",
                    choices = names(rawData()),
                    selected = names(rawData())[1])
  
  # Update variable choices for numeric columns only
  numeric_cols <- names(select_if(rawData(), is.numeric))
  updateCheckboxGroupInput(session, "variables",
                          choices = numeric_cols)
})
```

### Date Inputs

```r
# UI
dateInput("start_date", "Start date:", 
    value = Sys.Date() - 30),
dateRangeInput("date_range", "Date range:",
    start = Sys.Date() - 30,
    end = Sys.Date())
```

## Output Functions

### Tables

```r
# UI
tableOutput("static_table"),     # Static table
DT::dataTableOutput("interactive_table")  # Interactive table (requires DT package)

# Server
# Static table
output$static_table <- renderTable({
  req(processedData())
  head(processedData(), 10)
}, striped = TRUE, hover = TRUE)

# Interactive table - with pagination 
output$interactive_table <- DT::renderDataTable({
  req(processedData())
  processedData()
}, options = list(pageLength = 10, scrollX = TRUE))
```

### Text and Summaries

```r
# UI
verbatimTextOutput("summary"),
textOutput("status")

# Server
output$summary <- renderPrint({
  req(rawData())
  # Table summary example
  summary(rawData())
})

output$status <- renderText({
  req(rawData())
  # Text output sexample
  paste("Data loaded with", nrow(rawData()), "rows and", ncol(rawData()), "columns")
})
```

### Plots

```r
# UI
uiOutput("column_selector"),
selectInput("plot_type", "Choose column:", 
            choices = c("hist", "box"),
            selected = NULL),
plotOutput("plot1", 
        height = "400px")

# Server
output$plot1 <- renderPlot({
    # Choose what data to require
    req(rawData(), input$column)
    # Choose what data to use
    data <- rawData()

    if (input$plot_type == "hist") {
    ggplot(data, aes_string(x = input$column)) +
        geom_histogram(bins = input$bins, fill = "skyblue", alpha = 0.7) +
        theme_minimal() +
        labs(title = paste("Histogram of", input$column))
        
    } else if (input$plot_type == "box") {
    ggplot(data, aes_string(y = input$column)) +
        geom_boxplot(fill = "lightgreen", alpha = 0.7) +
        theme_minimal() +
        labs(title = paste("Boxplot of", input$column))
  }
})

output$column_selector <- renderUI({  
    # Choose what data to use
    data <- rawData()
    if (is.null(data)) return(NULL)  # Wait for data    
    numeric_cols <- names(data)[sapply(data, is.numeric)]  # Choose only numeric columns

    if (length(numeric_cols) == 0) {
        return(p("No numeric columns found in data."))
    }

    selectInput("column", "Select Column:", choices = numeric_cols)
})

```

## Layout Options

### Basic Layout

```r
# Simple fluid layout
ui <- fluidPage(
  titlePanel("My Shiny App"),
  
  sidebarLayout(
    sidebarPanel(
      # Input controls go here
      width = 4
    ),
    
    mainPanel(
      # Output displays go here
      width = 8
    )
  )
)
```

### Dashboard Layout

```r
library(shinydashboard)

ui <- dashboardPage(
  dashboardHeader(title = "My Dashboard"),
  
  dashboardSidebar(
    sidebarMenu(
      menuItem("Data Upload", tabName = "upload", icon = icon("upload")),
      menuItem("Analysis", tabName = "analysis", icon = icon("chart-line")),
      menuItem("Results", tabName = "results", icon = icon("table"))
    )
  ),
  
  dashboardBody(
    tabItems(
        tabItem(tabName = "upload",
        # Upload content
        ),
        tabItem(tabName = "analysis",
        # Analysis content
        ),
        tabItem(tabName = "results",
        # Results content
        )
    )
    )
)
```

### Tabbed Layout

```r
ui <- fluidPage(
  titlePanel("Multi-tab Application"),
    tabsetPanel(
        tabPanel("Data", 
        # Data upload and preview
        ),
        tabPanel("Analysis",
        # Analysis controls and outputs
        ),
        tabPanel("Visualization",
        # Plots and charts
        )
    )
)
```

## Advanced Features

### Conditional Panels

```r
# UI
conditionalPanel(
    condition = "input.show_advanced == true",
    # UI elements that only show when checkbox is checked
    numericInput("advanced_param", "Advanced parameter:", value = 1)
),

checkboxInput("show_advanced", "Show advanced options", value = FALSE)
```

### Notifications

```r
# Show different types of notifications
showNotification("Success!", type = "message")
showNotification("Warning!", type = "warning")
showNotification("Error occurred!", type = "error")
showNotification("Information", type = "message")
```

## Best Practices

1. **Always use `req()`** to check inputs before processing
2. **Handle errors gracefully** with `tryCatch()`
3. **Provide user feedback** with notifications and progress bars
4. **Use reactive values** for data that changes
5. **Keep server functions organized** and well-commented
6. **Test your app thoroughly** with different inputs
7. **Use meaningful variable names** and add comments

## Common Debugging Tips

- Use `print()` or `cat()` statements to debug server logic
- Check the R console for error messages
- Use browser() to pause execution and inspect variables
- Test reactive expressions in isolation
- Ensure all required packages are loaded

## Additional Resources
- [Shiny Gallery](https://shiny.rstudio.com/gallery/)
- [Shiny Application Layout Guide](https://www.appsilon.com/post/shiny-application-layouts)
