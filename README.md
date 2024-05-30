### [Documentation for Alma and Scientific Computing at the ICR](https://almacookbook.github.io)

The Alma CookBook contains recipes for using Alma, a high performance computing cluster at the ICR. The recipes are designed to be easy to follow and to provide a quick reference for common tasks. The recipes are written in markdown and can be viewed in a web browser or in a text editor.

The markdown is automatically rendered into an html site via a github action on pull to the main branch.  All changes should be made via a pull requests to the main branch. Let someone know if you make a change and need a reviewer. Once reviewed, you can perform the pull to main yourself.  

The site is hosted on github pages and can be accessed at [https://almacookbook.github.io](https://almacookbook.github.io)  

The markdown is built using mkdoc.  Information on how to use mkdoc can be found at [https://squidfunk.github.io/mkdocs-material/reference/diagrams/](https://squidfunk.github.io/mkdocs-material/reference/diagrams/)

The pages are built with markdown. Information on how to use markdown can be found at [https://guides.github.com/features/mastering-markdown/](https://guides.github.com/features/mastering-markdown/)  

The mkdocs.yml file is the configuration file for the site, in this you effectively define the sidebar and page links, along with theme and plugins.  The pages themselves are in the **docs** folder.  The site is built using the mkdocs.yml file and the pages in the docs folder, along with the github action located at .github/workflows/pages.yml.

