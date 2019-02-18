# nodejs-developer

    notes-node
        - Start and end with console.log statements to debug
        - Using File System and OS modules
        - nodejs.org/api lists npm packages
        - fs.appendFile() will append data to a file asynchronously
        - Use "const fs = require('fs')'" to import File System into fs var
        - Use "fs.appendFile('filename.ext', 'string', function(err){ anon function; });" to append text to file.
        - Use OS similarly
        - Template strings: `Hello ${user.username}!` will interpolate the string!
    
    Require your own file
        - Use require() much the same way, but with relative path.
            - require('./notes.js');
        - module gets exports property, which are items that can be used on the file requiring your module
            - module.exports.addnote = () => { function; }
    
    Require 3rd Party Modules from npm
        - Use npm init to create package.json file
        - We'll be using lodash
            - Installation    
                - npm install lodash --save
                - --save flag updates package.json
                - node_modules folder stores installed modules
                - package.json is updated with dependencies
                - can now "require('lodash');", typically stored in const "_"
            - Use
                - _.isString()
                - _.uniq(Array)
        - node_modules
            - Should not be uploaded to GitHub; use gitignore
            - npm install will load in dependencies from package.json

    Nodemon
        - Automatically restarts an app when the file is saved
        - CLI utility, installed and used a bit differently
            - npm install nodemon -g
                --g flag == global
            - nodemon filename.js == node filename.js
                - run files using nodemon, rather than node

    User Input
        - To access CLI args, use 'process.argv' array

    Yargs
        - Will make processing args way easier
        - Using yargs, we can parse CLI arguments a number of ways:
            - --title="secret"
            - --title secret
            - --title=secret
            - etc
        - Can replace command var def with command = argv._[0]

    Storing Notes in JSON
        - Converting object into string
            - JSON.stringify(object);
        - Converting string into object
            - JSON.parse('string');

    Adding Notes
        - Made reusable notes to read from file and write to file
            -   var fetchNotes = () => {
                    try {
                        var noteString = fs.readFileSync('notes-data.json');
                        return JSON.parse(noteString);
                    } catch (e) {
                        return [];
                    }
                };
            -   var saveNotes = (notes) => {
                    fs.writeFileSync('notes-data.json', JSON.stringify(notes));
                };
        - Made addNote() function
            -   var addNote = (title, body) => {
                    var notes = fetchNotes();
                    var note = {
                        title,
                        body
                    }

                    var duplicateNotes = notes.filter((note) => note.title === title);

                    if (duplicateNotes.length === 0) {
                        notes.push(note);
                        saveNotes(notes);
                        return note;
                    }
                };
        - Added some if/else for confirmation message.

    Removing a Note
        - In notes.js
            -   var notes = fetchNotes();
                var filteredNotes = notes.filter((note) =>  note.title !== title);
                saveNotes(filteredNotes);
                return notes.length !== filteredNotes.length;
        - In app.js
            -   var noteRemoved = notes.removeNote(argv.title);
                var message = noteRemoved ? 'Note was removed' : 'Note not found';
                console.log(message);
    Read

    Node Debugging (CLI)
        - node inspect filename.js --options
            - Enters debugger, can use debugger controls
                - n goes to next line
                - c goes to end or breakpoint
                - repl enters repl console
            - Putting a 'debugger;' line in your app will enter a breakpoint
                - you can then navigate to the breakpoint using c

    Node Debugging (Chrome)
        - node --inspect-brk filename.js --options
            - Enters debugger with hooks for Chrome
            - Can open '://inspect' in Chrome for debugger

    Listing All Notes
        - In notes.js
            -   getAll() returns fetchNotes()
        - In app.js
            -   var allNotes = notes.getAll();
                console.log(`Printing ${allNotes.length} note(s).`)
                allNotes.forEach((note) => notes.logNote(note));

    Requiring Arguments and Advanced Yargs
        - See yargs docs
        - .command() can configure the commands and whatnot
            -   const titleOptions = {
                    describe: 'Title of note',
                    demand: true,
                    alias: 't'
                }
            -   .command('add', 'Add a new note', {
                    title: titleOptions,
                    body: bodyOptions
                })

            -   var user = {
                    name: 'DISRUPTIVE',
                    sayHi: () => {
                        console.log(arguments); // Global
                        console.log(`Hi. I'm ${this.name}`);
                    },
                    sayHiAlt () {
                        console.log(arguments); // Function
                        console.log(`Hi. I'm ${this.name}`);
                    }
                }
                                            
    Arrow Functions
        - ES6 Feature
        - Normal syntax
            - var function = (args) => { function; };
        - Statement syntax
            - var function = (args) => function;
        - 1 argument, no parentheses
        - Do not bind a 'this' keyword.
            -   var user = {
                    name: 'DISRUPTIVE',
                    sayHi: () => {
                        console.log(`Hi. I'm ${this.name}`); // Global
                    },
                    sayHiAlt () {
                        console.log(`Hi. I'm ${this.name}`); // Function
                    }
                }
        - Does not bind arguments array
