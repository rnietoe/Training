# Configure Tooling for managing documentation

## Installing MkDocs

MkDocs is required https://www.mkdocs.org/. And the installation guide can be found https://www.mkdocs.org/#installation. 

* Install python (https://www.python.org/)
* Install mkdocs

  ```python
  pip install mkdocs
  ```  

* Install mkdocs material theme https://squidfunk.github.io/mkdocs-material/

  ```python
  pip install mkdocs-material      
  ```

* Install mkdocs pdf plugin

  ```python
  pip install mkdocs-pdf-export-plugin 
  ```

* Install pygments (http://pygments.org) & pymdown-extension (https://squidfunk.github.io/mkdocs-material/extensions/pymdown/)

  ```python
  pip install pygments
  
  pip install pymdown-extensions
  ```

## Executing mkdocs 

For local testing you can run a `mkdocs serve` and test your site at http://localhost:8000

For building documentation you can:
    
- Generate MkDocs:  `mkdocs build`
