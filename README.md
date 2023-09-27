# pylitools-ws-20231122

## TODO: Präsentation

- docker run -it -u $(id -u):$(id -g) -v $(pwd):/documents/ asciidoctor/docker-asciidoctor
- https://github.com/asciidoctor/docker-asciidoctor/blob/main/README.adoc
- asciidoctor-revealjs -a revealjsdir=https://cdn.jsdelivr.net/npm/reveal.js slides.adoc

- docker run --rm -it -v $(pwd):/workdir danteev/texlive latexmk -pdfxe foo.tex
- Vorstellen der Variante mit paar Erläuterungen

- asciidoctorj -a revnumber=0.0.99 -b pdf index.adoc

- Schlussfolie Vor- und Nachteile:
  * native:  
    - man muss eine API schreiben (zusätzlich zur shared library).
    - Man kann "normale" Python-Distribution verwenden. -> Grösstmögliche Kompatibilität. 
    - shared lib ist OS-abhängig
    - start up Zeit ist besser
    - Validierungs-Performance jedoch schlechter


## Vorbereitung

### Multipass
Mit [Multipass](https://multipass.run/install) können einfach und schnell Linux-VM erstellt werden. Version 1.11.1 funkioniert gut, spätere haben bei mir Probleme verursachte (macOS).

VM (Ubuntu 22.04) erstellen und starten:
```
multipass launch jammy --cpus 4 --disk 20G --memory 8G --name pylitools
```

Verzeichnis mounten:
```
multipass mount $PWD pylitools:/home/ubuntu/pylitools
```

Robuster (symlinks etc.) ist das Clonen des Repos innerhalb der VM: 

```
git clone https://github.com/edigonzales/pylitools-ws-20231122.git
```

In VM einloggen:
```
multipass shell pylitools
```

VM stoppen:
```
multipass stop pylitools
```

VM anzeigen:
```
multipass list
```

VM definitiv löschen:
```
multipass delete --purge pylitools
```

### Java installieren

Auf Ubuntu und Linux mit [sdkman](https://sdkman.io/):

```
sudo apt-get install zip unzip
curl -s "https://get.sdkman.io" | bash
source "/home/ubuntu/.sdkman/bin/sdkman-init.sh"
```

Java installieren:

```
sdk l java
```

```
sdk i java 11.0.20.1-tem
java -version
```

Wird für Jython benötigt und für einen Schritt mit GraalPy.