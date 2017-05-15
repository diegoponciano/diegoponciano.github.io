# Distribuindo sua lib Python pelo PyPI

** Esse artigo é uma tradução livre de http://peterdowns.com/posts/first-time-with-pypi.html **

Uma das coisas que mais me chamou atenção quando ingressei no mundo Python, foi a facilidade de usar libs feitas por outras pessoas.  
Além da enorme quantidade de libs disponíveis, basta rodar `pip install nome_da_lib` e você pode usá-la no seu projeto.  
Resolvi criar esse guia pra mostrar como distribuir as suas próprias libs, e contribuir com a comunidade. :D

### Quem é o [PyPI](http://pypi.python.org/)?
PyPI é a sigla de *Python Package Index*, ou **Índice de Pacotes Python**, e é o nome do principal site que hospedas libs Python.  
Do site oficial:  

> The Python Package Index is a repository of software for the Python programming language.  

Traduzindo:  
> O *Índice de Pacotes Python* é um repositório de software para a linguagem de programação Python.  

Você 


Written something cool? Want others to be able to install it with easy_install or pip? Put your code on PyPI. It's a big list of python packages that you absolutely must submit your package to for it to be easily one-line installable.

The good news is that submitting to PyPI is simple in theory: just sign up and upload your code, all for free. The bad news is that in practice it's a little bit more complicated than that. The other good news is that I've written this guide, and that if you're stuck, you can always refer to the official documentation.

I've written this guide with the following assumptions:

The module/library/package that you're submitting is called mypackage.
mypackage is hosted on github.
Create your accounts

On PyPI Live and also on PyPI Test. You must create an account in order to be able to upload your code. I recommend using the same email/password for both accounts, just to make your life easier when it comes time to push.

Create a .pypirc configuration file

This file holds your information for authenticating with PyPI, both the live and the test versions.

[distutils]
index-servers =
  pypi
  pypitest

[pypi]
repository=https://pypi.python.org/pypi
username=your_username
password=your_password

[pypitest]
repository=https://testpypi.python.org/pypi
username=your_username
password=your_password
This is just to make your life easier, so that when it comes time to upload you don't have to type/remember your username and password. Make sure to put this file in your home folder – its path should be ~/.pypirc.

Because this file holds your username and password, you may want to change its permissions so that only you can read and write it. From the terminal, run:

chmod 600 ~/.pypirc
Thanks to Martin Schobert for the recommendation.

Notes on passwords / usernames

Michiel Sikma has reported that in Python 3 if your password includes a raw %, it needs to be escaped by doubling – the .pypirc config parser interpolates strings. For example, if your password is hello%world:

[pypi]
repository=https://pypi.python.org/pypi
username=myusername
password=hello%%world
I've never run into this issue, but if you're having trouble this might help.

James Stidard has reported that this escaping behavior has been patched and is no longer necessary. If you're seeing an error with a response code of 403: Invalid or non-existent authentication information, try un-escaping the percent signs in your password.

Andrew Farrell points out that if your password includes spaces, make sure not to quote it. For example, if your password is correct horse battery staple:

[pypi]
repository=https://pypi.python.org/pypi
username=myusername
password=correct horse battery staple
Thanks to Michiel, Andrew, and Charlie Hack for their help with this section.

Prepare your package

Every package on PyPI needs to have a file called setup.py at the root of the directory. If your'e using a markdown-formatted read me file you'll also need a setup.cfg file. Also, you'll want a LICENSE.txt file describing what can be done with your code. So if I've been working on a library called mypackage, my directory structure would look like this:

root-dir/   # arbitrary working directory name
  setup.py
  setup.cfg
  LICENSE.txt
  README.md
  mypackage/
    __init__.py
    foo.py
    bar.py
    baz.py
Here's a breakdown of what goes in which file:

setup.py

This is metadata about your library.

from distutils.core import setup
setup(
  name = 'mypackage',
  packages = ['mypackage'], # this must be the same as the name above
  version = '0.1',
  description = 'A random test lib',
  author = 'Peter Downs',
  author_email = 'peterldowns@gmail.com',
  url = 'https://github.com/peterldowns/mypackage', # use the URL to the github repo
  download_url = 'https://github.com/peterldowns/mypackage/archive/0.1.tar.gz', # I'll explain this in a second
  keywords = ['testing', 'logging', 'example'], # arbitrary keywords
  classifiers = [],
)
The download_url is a link to a hosted file with your repository's code. Github will host this for you, but only if you create a git tag. In your repository, type: git tag 0.1 -m "Adds a tag so that we can put this on PyPI.". Then, type git tag to show a list of tags — you should see 0.1 in the list. Type git push --tags origin master to update your code on Github with the latest tag information. Github creates tarballs for download at https://github.com/{username}/{module_name}/archive/{tag}.tar.gz.

Thanks to Lars Blumberg for writing in about Github's current archive URL scheme.

setup.cfg

This tells PyPI where your README file is.

[metadata]
description-file = README.md
This is necessary if you're using a markdown readme file. At upload time, you may still get some errors about the lack of a readme — don't worry about it. If you don't have to use a markdown README file, I would recommend using reStructuredText (REST) instead.

LICENSE.txt

This file will contain whichver license you want your code to have. I tend to use the MIT license.

Upload your package to PyPI Test

Run:

python setup.py register -r pypitest
This will attempt to register your package against PyPI's test server, just to make sure you've set up everything correctly.

Then, run:

python setup.py sdist upload -r pypitest
You should get no errors, and should also now be able to see your library in the test PyPI repository.

Upload to PyPI Live

Once you've successfully uploaded to PyPI Test, perform the same steps but point to the live PyPI server instead. To register, run:

python setup.py register -r pypi
Then, run:

python setup.py sdist upload -r pypi
and you're done! Congratulations on successfully publishing your first package!










Dillinger is a cloud-enabled, mobile-ready, offline-storage, AngularJS powered HTML5 Markdown editor.

  - Type some Markdown on the left
  - See HTML in the right
  - Magic

# New Features!

  - Import a HTML file and watch it magically convert to Markdown
  - Drag and drop images (requires your Dropbox account be linked)


You can also:
  - Import and save files from GitHub, Dropbox, Google Drive and One Drive
  - Drag and drop markdown and HTML files into Dillinger
  - Export documents as Markdown, HTML and PDF

Markdown is a lightweight markup language based on the formatting conventions that people naturally use in email.  As [John Gruber] writes on the [Markdown site][df1]

> The overriding design goal for Markdown's
> formatting syntax is to make it as readable
> as possible. The idea is that a
> Markdown-formatted document should be
> publishable as-is, as plain text, without
> looking like it's been marked up with tags
> or formatting instructions.

This text you see here is *actually* written in Markdown! To get a feel for Markdown's syntax, type some text into the left window and watch the results in the right.

### Tech

Dillinger uses a number of open source projects to work properly:

* [AngularJS] - HTML enhanced for web apps!
* [Ace Editor] - awesome web-based text editor
* [markdown-it] - Markdown parser done right. Fast and easy to extend.
* [Twitter Bootstrap] - great UI boilerplate for modern web apps
* [node.js] - evented I/O for the backend
* [Express] - fast node.js network app framework [@tjholowaychuk]
* [Gulp] - the streaming build system
* [Breakdance](http://breakdance.io) - HTML to Markdown converter
* [jQuery] - duh

And of course Dillinger itself is open source with a [public repository][dill]
 on GitHub.

### Installation

Dillinger requires [Node.js](https://nodejs.org/) v4+ to run.

Install the dependencies and devDependencies and start the server.

```sh
$ cd dillinger
$ npm install -d
$ node app
```

For production environments...

```sh
$ npm install --production
$ npm run predeploy
$ NODE_ENV=production node app
```

### Plugins

Dillinger is currently extended with the following plugins. Instructions on how to use them in your own application are linked below.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md] [PlDb] |
| Github | [plugins/github/README.md] [PlGh] |
| Google Drive | [plugins/googledrive/README.md] [PlGd] |
| OneDrive | [plugins/onedrive/README.md] [PlOd] |
| Medium | [plugins/medium/README.md] [PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md] [PlGa] |


### Development

Want to contribute? Great!

Dillinger uses Gulp + Webpack for fast developing.
Make a change in your file and instantanously see your updates!

Open your favorite Terminal and run these commands.

First Tab:
```sh
$ node app
```

Second Tab:
```sh
$ gulp watch
```

(optional) Third:
```sh
$ karma test
```
#### Building for source
For production release:
```sh
$ gulp build --prod
```
Generating pre-built zip archives for distribution:
```sh
$ gulp build dist --prod
```
### Docker
Dillinger is very easy to install and deploy in a Docker container.

By default, the Docker will expose port 80, so change this within the Dockerfile if necessary. When ready, simply use the Dockerfile to build the image.

```sh
cd dillinger
docker build -t joemccann/dillinger:${package.json.version}
```
This will create the dillinger image and pull in the necessary dependencies. Be sure to swap out `${package.json.version}` with the actual version of Dillinger.

Once done, run the Docker image and map the port to whatever you wish on your host. In this example, we simply map port 8000 of the host to port 80 of the Docker (or whatever port was exposed in the Dockerfile):

```sh
docker run -d -p 8000:8080 --restart="always" <youruser>/dillinger:${package.json.version}
```

Verify the deployment by navigating to your server address in your preferred browser.

```sh
127.0.0.1:8000
```

#### Kubernetes + Google Cloud

See [KUBERNETES.md](https://github.com/joemccann/dillinger/blob/master/KUBERNETES.md)


### Todos

 - Write MOAR Tests
 - Add Night Mode

License
----

MIT


**Free Software, Hell Yeah!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>
