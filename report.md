## Report für SEW TEST
Das hier ist ein Report MD file


##flask vue crud
#Definieren Sie einen Test-Endpunkt auf localhost mit der Port Nummer 8080. Was müssen Sie dafür beim Flask-Server konfigurieren?

app.run(port=8080)
port im app run hinzufügen


#Implementieren Sie eine TODO-Liste mit Flask mit folgenden Elementen: {id, todo, assignee, done}. Was haben Sie geändert oder welche Elemente haben Sie neu definiert?

Refactor mit Match-Case in app.py
title -> todo
author -> assignee
read -> done
Book -> Todo
BOOKS -> TODOS
book -> todo

Die app.routes müssen geändert werden!

STRG F "price" alles mit price entfernen, beistiriche nicht vergessen

charge methoden entfernen

app route zu remove_book hinzugefügt


#Bereiten Sie die grafische Oberfläche für eine einfache Erstellung, Anzeige, Löschung und Anpassung der TODOs vor. Welche Komponenten müssen dafür erstellt werden?

Refactor mit Match-Case in app.py
title -> todo
author -> assignee
read -> done
Book -> Todo
BOOKS -> TODOS
book -> todo

Purchase STR-F
Purchase entfernen

price entfnernen

localhost port auf 8080 changen refrencen von büchern auf todo nach server file

#Ermöglichen Sie die einfache Erweiterung der grafischen Oberfläche und beschreiben Sie notwendige Schritte um neue Komponenten zur Anmeldung oder persönlichen Definition von personenbezogenen TODOs zu ermöglichen.

Abstände zwischen tds, da neue hinzufügen




##flask vue spa

#Welche Tools würden Sie einsetzen, und wie würden die entsprechenden Konfigurationsdateien aussehen? Erstellen Sie ein Konzept!
Ich würde tox mit pytest und cypress verwenden für testen pytest für unit tests und cypress für view tests, das ganze kann mit travis CI automatisieren

travis.yml mit language:python

seutup.py

tox.ini mit

[tox]
envlist = py34

[testenv]
deps = -r requirements.txt
commands =
    pytest --cov=server --html=testreport.html --self-contained-html

[pytest]
testpaths = src/unittest/python
python_files = test_*.py
python_classes = Test




#Bereiten Sie einen einfachen Test für den Aufruf der Random Funktion vor. Wie würden Sie diesen starten?

client.get vorbereiten

#Frage 6
