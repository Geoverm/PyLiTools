# GraalPy

GraalPy ist eine Python-Implementierung. Dank GraalVM lässt sich damit Polyglot programmieren und es lassen sich neben anderen Sprachen Java-Klassen ausführen (ähnlich jython). 

## GraalPy installieren

Siehe https://github.com/oracle/graalpython/releases

```
wget https://github.com/oracle/graalpython/releases/download/graal-23.1.0/graalpy-community-jvm-23.1.0-linux-amd64.tar.gz
wget https://github.com/oracle/graalpython/releases/download/graal-23.1.0/graalpy-community-jvm-23.1.0-linux-aarch64.tar.gz
```

```
tar -xzf graalpy-community-jvm-23.1.0-linux-amd64.tar.gz -C /home/ubuntu/apps/
tar -xzf graalpy-community-jvm-23.1.0-linux-aarch64.tar.gz -C /home/ubuntu/apps/
```
Es muss die "-jvm"-Version sein. Die non-community-Version dünkt mich broken.

Python im Pfad ergänzen, z.B. _.bashrc_. Falls vorgängig das Jython-Beispiel durchgespielt wurde, muss Jython wieder aus dem PATH entfernt werden:

```
which python
python --version
```

## Venv erstellen

In das _graalvm_-Verzeichnis wechseln.

```
cd pylitools-ws-20231122
cd graalvm
```

```
graalpy --python.Executable=/home/ubuntu/apps/graalpy-community-23.1.0-linux-amd64/bin/graalpy -m venv .venv
graalpy --python.Executable=/home/ubuntu/apps/graalpy-community-23.1.0-linux-aarch64/bin/graalpy -m venv .venv
```

`--python.Exectuable=...` ist notwendig wegen Bug (nur auf Linux ARM64).


Und aktivieren:
```
source .venv/bin/activate
```

## Einfaches Python-Skript ausführen

```
graalpy hello.py
```

Weil wieder eine JVM (und ein [Context](https://www.graalvm.org/sdk/javadoc/org/graalvm/polyglot/Context.html)?) gestartet wird, ist die Startup-Zeit schlechter. 

## Java-Klassen in Python-Skript verwenden

```
python3 hello_java.py
```

## Ilivalidator in Python-Skript verwenden

Wie in Jython muss man dem Python-Skript die Java-Libraries heruntergeladen werden. Es gibt keine "built-in"-Möglichkeit. Man kann dazu ein Build-Tool (aus der Java-Welt) verwenden:

```
./gradlew getDeps
```

Die Java-Klassen müssen im Python-Code dem Klassenpfad hinzugefügt werden:

```
files = os.listdir(os.path.join(".venv", "javalib"))
for file in files:
    if file.endswith("jar"):
        java.add_to_classpath(os.path.join(".venv", "javalib", file))
```

Anschliessend kann das Skript ausgeführt werden:

```
graalpy validate.py
```

## Minimaler Validator-Webservice mit Flask

Flask installieren:
```
pip install Flask
```

Webservice starten:
```
graalpy webservice.py
```

Falls in einer VM gearbeitet wird, muss diese ggf so konfiguriert werden, dass man via Browser auf sie zugreifen kann. Die IP mit Multipass kann man wie folgt herausfinden:
```
multipass list
```

Anschliessend z.B. `http://192.168.64.2:5001/`. Falls alles lokal ausgeführt wird, reicht `http://0.0.0.0:5001/`.
