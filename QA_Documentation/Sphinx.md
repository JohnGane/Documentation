**Доступ к GIT с документацией RTT**

[https://gitlab1.rtt.rusteletech.ru/](https://gitlab1.rtt.rusteletech.ru/) (172.16.2.54)

schernykh

qwer1234

**Компиляция документации html с текущими версиями пакетов на сервере Ubuntu_Server**

cd REPO_Documentation/RTT_Documentation/rtt-portal/

source .env/bin/activate

sphinx-build -b html source/ build/html


**Создание документации на NGFW Halo**

Используется: 🟦Версия sphinx 6.2.1  
🟦Версия sphinx-build 6.2.1  
🟦Версия sphinxawesome-theme 5.0.0b5

Папка с собранными документами "build" игнорируется


**Для сборки документации**

`#HTML`
 
`sphinx-build -b html docs/source/ docs/build/html` 

`#PDF`

`sphinx-build -b latex docs/source/ docs/latex/`
`cd docs/latex/` 
`pdflatex -interaction=nonstopmode manual.tex` 
`makeindex manual.idx` 
`pdflatex -interaction=nonstopmode manual.tex`

 `#Word`

`sphinx-build -b singlehtml -D html_theme=basic docs/source/ docs/build/singlehtml`
`cd docs/build/singlehtml`
`pandoc -o index.docx index.html`

docs/source/index.rst - файл-исходник  
conf.py - конфиг

**Установка необходимых пакетов**


#`sphinx`

`apt-get install python3-sphinx`

#`latex (pdf)`

`sudo apt-get install  texmaker gummi texlive texlive-full texlive-latex-recommended latexdraw intltool-debian lacheck libgtksourceview2.0-0 libgtksourceview2.0-common lmodern luatex po-debconf tex-common texlive-binaries texlive-extra-utils texlive-latex-base texlive-latex-base-doc texlive-luatex texlive-xetex texlive-lang-cyrillic texlive-fonts-extra texlive-science texlive-latex-extra texlive-pstricks`

#`awesome-theme(development version - 5)`

`pip install git+https://github.com/kai687/sphinxawesome-theme.git`