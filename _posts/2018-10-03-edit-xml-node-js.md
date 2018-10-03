---
layout: post
title: How to Read, Write, and Update XML with Node.js
description: How to read, write, and update XML with Node.js
keywords: node, node.js, xml, node write xml, node read xml, node update xml, javascript, javascript xml, node.js xml, node xml
author: curtis_larson
comments: true
---

## Introduction

Often times I have to make small edits to XML files provided by clients due to various errors that are in the file or updates that need to be made to the data. Rather than manually edit potentially thousands of lines of XML I usually write a quick script to do the changes automatically. I'll show you how to easily read, write, and update xml using Node.js and a useful npm package.

## Setup

First off you need to create a directory, run `npm init` and then run `npm install --save xml2json` to install the [xml2json](https://www.npmjs.com/package/xml2json) package which will be doing the bulk of the work. Make sure to also create a main JavaScript file (like `index.js`) and have your `package.json` point to it correctly. Here's an example of an xml file that needs to be edited:

    <Survey>
      <!-- QUESTION 1 -->
      <Question QuestionId='1'>
        <QuestionText>What is the air speed velocity of an unladen swallow?</QuestionText>
      </Question>

        <!-- ANSWER 1 -->
      <Answer QuestionId='1' AnswerTypeId='1' DisplayOrder='1' AnswerId='286'>
        <AnswerText>12?</AnswerText>
      </Answer>

        <!-- ANSWER 2 -->
      <Answer QuestionId='1' AnswerTypeId='1' DisplayOrder='2' AnswerId='286'>
        <AnswerText>African or European</AnswerText>
      </Answer>

        <!-- ANSWER 3 -->
      <Answer QuestionId='1' AnswerTypeId='1' DisplayOrder='3' AnswerId='286'>
        <AnswerText>11 meters per second.</AnswerText>
      </Answer>

        <!-- ANSWER 4 -->
      <Answer QuestionId='1' AnswerTypeId='1' DisplayOrder='4' AnswerId='286'>
        <AnswerText>I don’t know.</AnswerText>
      </Answer>
    </Survey>

As you can see in the XML file the `AnswerId` property is repeated for each answer. This was causing problems with the import script that needed to generate a survey from the XML.

## Reading and Parsing the XML

The first step was reading the xml file, which I dropped into the same folder as my main JavaScript file and aptly named it `survey.xml`. Reading the xml file is as easy as importing the `fs` package and using the `readFile` function:

    var fs = require('fs');

    fs.readFile( './survey.xml', function(err, data) {

    });

Now that we have access to the raw xml data we can throw it into the `xml2json` parser:

    var fs = require('fs');
    var parser = require('xml2json');

    fs.readFile( './survey.xml', function(err, data) {
      var json = JSON.parse(parser.toJson(data, {reversible: true}));
    });

Using the `toJson` function allows us to convert a raw XML string into a stringified JSON object. Using `JSON.parse` will convert that string into a JavaScript object that we can manipulate. Notice the option of `{reversible: true}` that I passed into the `toJson` function. This allows us to convert the JSON we manipulate back to conformant XML.

## Editing the XML

As mentioned above, we want to make sure that the `AnswerId` property is unique for each `Answer`. We can do this by looping through each of the answers and setting it's `AnswerId` property to the index of the loop as so:

    var fs = require('fs');
    var parser = require('xml2json');

    fs.readFile( './survey.xml', function(err, data) {
      var json = JSON.parse(parser.toJson(data, {reversible: true}));
      var answers = json["Survey"]["Answer"];
      for (var i = 0; i < answers.length; i++) {
        var answer = answers[i];
        answer.AnswerId = i;
      }
    });

Notice that I accessed `json["Survey"]["Answer"]` and it returned an array of objects. The JSON object is structured so that each XML element is nested inside it's parent element as an object property. Elements that are at the same level are just added to an array rather than as separate properties. This allows us to access all Answers at once that are a child of the `Survey` element.

## Converting back to XML and Writing

Now that we have made the necessary change we can use `xml2json.toXml` to convert the modified JSON back into xml which we can write back to a file.

    fs = require('fs');
    var parser = require('xml2json');

    fs.readFile( './survey.xml', function(err, data) {
      var json = JSON.parse(parser.toJson(data, {reversible: true}));
      var answers = json["Survey"]["Answer"];
      for (var i = 0; i < answers.length; i++) {
        var answer = answers[i];
        answer.AnswerId = i;
      }

      var stringified = JSON.stringify(json);
      var xml = parser.toXml(stringified);
      fs.writeFile('survey-fixed.xml', xml, function(err, data) {
        if (err) {
          console.log(err);
        }
        else {
          console.log('updated!');
        }
      });
    });

We need to make sure to call `JSON.stringify` on the JavaScript object before we pass it to the `toXML` function (which expects a string). Since we used the `reversible` property we should now have a fully compliant XML file that has fixed `AnswerId` properties. That's all you need to know for reading, updating, then writing back an XML file with Node.js. Feel free to leave a comment or ask any questions!
