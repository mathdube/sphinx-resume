# sphinx-resume

## What is this project? 

I wanted to experiment with a few components by doing a project that would also potentially be useful so I decided to redesign my resume using Sphinx and Python.

The idea was to see if I could rebuild my resume with a configuration as code and devops approach.
### Bonus advantages

Even though this was more of a little challenge to see how I could use the technology I (half) knew there are still a couple of neat features to this approach

- Decouples the data and design so I can build multi-language out of the box.
- Restructured text is fairly lightweight and fits the typical resume format pretty well out of the box.
- I can rebuild my resume from anywhere using could build tools.

## Components

### Python

https://www.python.org/

The basics are in Python 3 and the build steps with sphinx leverage Python. The actual custom code is actually very small.  

### Sphinx

https://www.sphinx-doc.org/

Sphinx is typically used to package documentation with the project source code when building Python apps.

Here it'll interpret the restructured text of index.rst and run the python code in conf.py to contruct the html pages.

### Jinja2 

https://jinja.palletsprojects.com/en/2.11.x/

Jinja is a templating language that allows the project to avoid repeating the same code. 

As good as Restructured Text and Sphinx are at building a page from barebones intructions they tend to repeat themselves
 for sections that appear multiple times.
 
 Jinja2 fixes this by templating those sections with loops.

### YAML

https://yaml.org/

Almost all of the text shown on the pages is contained inside YAML files which are loaded and replaced using Jinja2.

I did this so I didn't have to repeat section, but also to build multiple languages with the same design without
changing anything.

### Github

https://github.com/

https://pages.github.com/

GitHub hosts the source code and I also redeploy the artifacts back to GitHub Pages. 

The pages build by this are static and GitHub Pages is a free and easy hosting service so I
 just put back the page back there.

### AWS CodeBuild

https://aws.amazon.com/codebuild/

After building the project I thought it would be cool to get it to deploy on the cloud so I could run a build from
 anywhere without worrying about prerequisites or OS.

## Steps

### Load, template and render with Sphinx

Sphinx first glues a few components together by running the following command:

`make html`

- First it'll pick the format from index.rst. You'll notice that most of this is Jinja2 template instructions.

- Then python code in conf.py is run. I had a bit of fun trying to get certain values dynamically but overall
the process mainly loads the data from the yaml file and runs the templating on the context from the index.rst file.


``` python
    with open(yml_file, 'r', encoding='utf8') as f:
        cv_data = yaml.safe_load(f)
    html_context["cv_data"] = cv_data
    src = source[0]
    rendered = app.builder.templates.render_string(
        src, app.config.html_context
    )
```

- From the make file I run this process twice, once in french and then in english. It's a bit of a hack for a 
multilingual build

```bash
@$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O) -E -D language=fr
cp $(BUILDDIR)/html/index.html $(BUILDDIR)/html/index_fr.html
@$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O) -E -D language=en
```

- The resulting pages are placed in docs/ and can be pushed back to GitHub Pages as built.

### Build and deploy on cloud.

The whole set of steps can be achieved locally, but I thought that down the road it might be fun to not have to worry
about where I'm building this, it's also a cool way to try out AWS CodeBuild.

- **Build instructions** Instructions on how to build are put in the `buildspec.yml` file.
When the AWS CodeBuild project is setup and run this file will be loaded from source code and executed by CodeBuild.
Typically they're commands or in some cases packages to install as prereqs. 
This is done in four stages: install, pre-build, build and post-build.

- **Python prerequisites** `requirements.txt` contains the python packages that we need to build a successful project.
To install them I setup a python venv and do a pip install from the `requirements.txt` file

- **Buidling** `make html` is invoke just like in the previous section by CloudBuild 
as per the instructions in the `buildspect.yaml` to create the artifacts in docs/.

- **Deploying** : I setup a deploy key in GitHub and placed that key in a dedicated S3 bucket accessible to my AWS CodeBuild project.
Once the artifacts are ready, in the post-build I just pull the key, add docs/ to the 
commit and push it back to GitHub Pages.
