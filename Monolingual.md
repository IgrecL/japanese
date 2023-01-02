# Optimise Yomichan for monolingual dictionaries

Adding furigana to Japanese fields can be automated with the [AJT Furigana](https://github.com/Ajatt-Tools/Furigana) addon : you have to create temporary fields that contain the sentences without furigana, and then configure the source/destination fields in AJT Furigana parameters.<br>
Once configured correctly, AJT Furigana will generate the furigana fields upon new card creation.<br><br>
<img src="https://user-images.githubusercontent.com/99618877/208629873-460ad796-15fa-427c-aa72-3cb2b8f2bbee.png" width="200"><br>

The next step towards automation when using a monolingual dictionary is deleting the first line of the definition entry extracted by Yomichan from the Japanese dictionaries. As you can see here, the first line にほん‐ご【日本語】is hardcoded inside the definition, so it cannot be achieved without the following trick.<br>
<img src="https://user-images.githubusercontent.com/99618877/208631563-8566757c-cd68-4f9c-8faf-e563dcfa1938.png" width="500">
<img src="https://user-images.githubusercontent.com/99618877/208632803-b376eb13-4e41-4a68-a2a1-b0a02013a004.png" width="500">
<br>

My solution here is directly changing the way Yomichan interprets the dictionary entry.<br>
To do so you have to edit the [Handlebars.js](https://handlebarsjs.com/guide/) code in the "Configure Anki card templates..." section of Yomichan parameters (Advanced mode needs to be switched on)<br>
This can be done using the regexReplace segment to delete everything until the first newline and the newline itself.
```
{{~#*inline "glossary"~}}
    {{~#scope~}}
        {{~#if (op "===" definition.type "term")~}}
            {{#regexReplace "^[^<br>]+<br>|「.{1,10}」|（.{1,20}）" ""}}
                {{~> glossary-single definition brief=true noDictionaryTag=true ~}}
            {{/regexReplace}}
```
It also deletes the「」and（）brackets, which are used to specify the word class and give examples, but I don't find them useful.

Result:
![image](https://user-images.githubusercontent.com/99618877/208634851-1588fc58-506b-4d25-a689-d3e3a6b4cff8.png)
