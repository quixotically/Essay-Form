# Essay Form

## Technologies
I used Formspree to send emails without a backend, and CKEditor to add  word-processing features to the textarea. CKEditor is an amazing tool that is very customizable - check it out! I added the WordCount plugin and removed a few of the included plugins of the full preset to make the customized editor you see in this project. I attempted to use the Autosave plugin as well, but I could not get it to work. But, I was able to implement autosave myself using HTML5's localStorage!

## TODO
- change font
- style button and error/save
- test in all browsers
- test quitting out of browser
- add tab listener to essay area
- add meta tag for mobile
- implement spinner
- add favicon
- test in subway
- refactor!

## What senders will want to know
- If they reply to their own message, they will be replying to themselves! This is just a quirk of using the same cc email as the reply_to email. But, if the form recipient is the one sending the message, the message recipient will be as expected since the reply_to email was designated from the form recipient's point of view.
- They can probably use this on the subway! HTML5 woot! Just tell them to make sure to only submit when they have signal again.
- Autosave is on the browser, not across devices.
- Not being able to tab is annoying. There is a misleading button on the editor that seems like it will tab, but it just ends up tabbing the whole paragraph. Make sure they manually type in spaces for the tab effect.

## Features

## Testing Formspree
I realized that Formspree would not work if I tried to submit the form locally. I created a gh-pages branch and pushed to the corresponding remote (which I named Test, which I will now use for testing future projects that require a real domain) to test Formspree.

## HTML5 Storage
An amazing feature of HTML5 that allows the user to effectively save data onto the browser. No backend required! It even works if you quit out of the browser, as long as you use localStorage instead of sessionStorage. This was a valuable example for me: https://github.com/mdn/web-storage-demo/blob/gh-pages/main.js
And the autosave plugin code turned out to be useful too: https://github.com/w8tcha/CKEditor-AutoSave-Plugin/blob/master/autosave/plugin.js

## Configuring the WordCount plugin
I went into the ckeditor.js file and set maxWordCount to 300 and hardLimit to false to prevent the editor from preventing you to type more than 300 words (if you did this using the minified version, change the 0 to a 1). I also changed the < to a <= when checking for the word and char count limits in the else if statement, so that the limitRestored event would fire when the wordCount was equal to the maxWordCount. Otherwise, if the user types above the limit and then deletes until they reach the limit, the wordCount will still show up red. I did not bother with changing the wordCount display to green when the wordCount exactly matched the limit, since the display contains the paragraph count as well. A green, incorrect paragraph count could confuse users.

Since I could not find a way to get the paragraph count in the API documentation or plugin documentation, I had to manually do this in a script. There is at least a way to get the html content of the replaced textarea, which is ```<editor instance>.getData()```. With this, I just wrapped the string in a div in order to use ```find()```. The result looks like this:
```
$('<div>' + essayHTML + '</div>').find("p").length
```

## Using a downloaded font
1. Download the font you want into your project
2. Convert the ttf file to eof in order for your font to be compatible with IE and insert that into the same folder as the original. This is a free tool to do that: https://www.kirsle.net/wizards/ttf2eot.cgi
3. Insert this CSS at the top
```
@font-face {
 font-family: anyname;
 src: url("path/file.eot");
}
@font-face {
 font-family: anyname;
 src: url("path/file.ttf");
}
```
4. Set ``` font-family: "anyname" ``` on the desired elements!

## Errors
There is an error that pops up in the Chrome console when you do not fill out all of the required input fields. It looks like this:
```
An invalid form control with name='' is not focusable.
```
I was able to figure out that this is due to the ```<input required>``` fields not being direct children of the ```<form>```. Luckily, these errors do not affect the functionality of this application.
