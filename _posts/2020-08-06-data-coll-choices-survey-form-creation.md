---
layout: post
title : "Survey form creation"
subtitle : "choosing between the ODK/KoboToolbox & Google Forms"
date: 2020-08-06 09:14:00
author: "Craig Dsouza"
header-img: "img/posts/imis/banner.png"
comments: true
tags: [odk, kobo, google-forms]
category: [blog, data-how-tos]
image_sliders:
  - form_builders_1
---

Data collection platforms consist of multiple components
- survey form formats & form builders
- data collection app
- data storage
- data visualization

In this post we will cover survey form formats & form builders, and do a pointwise comparison of the major free alternatives,
Open Data Kit(ODK)/KoboToolbox & Google Forms. Future blogposts may go into detail on each of these individually. I considered Survey Monkey too, but it's use cases are focused heavily on companies and market research, which would take this blogpost off on a tangent, and hence I chose to leave it out for now.

Table of Contents
===================
0. [Survey Form formats](#0-survey-form-formats)
1. [An Overview of the features of form builders](#1-an-overview-of-the-features-of-form-builders)
   1. [Desktop & Web formats](#1a-desktop-and-web)
   2. [Question types & Metadata questions](#1b-question-types-and-metadata-questions)
   3. [Composite questions, Groups and Repeats](#1c-composite-questions-groups-and-repeats)
   4. [Response Validation and Form Logic](#1d-response-validation-and-form-logic)
   5. [Dynamic calculations](#1e-dynamic-calculations)
   6. [Survey templates](#1f-survey-templates)
   7. [Form styling](#1g-form-styling)
   8. [Multi-lingual forms](#1f-multi-lingual-forms)

# 0. Survey Form formats
An XForm (*.xml) is the open Survey Form format in which ODK/Kobo forms are created. You can create XForms using form builders such as ODK Build,
or Kobo Build. Another way to create XForms is by first creating XLSForms, and then converting these to XForms using appropriate [tools](https://getodk.org/xlsform/)
XLSForms are in fact analogous to XForms, except that they can be read and created in Microsoft Excel or other spreadsheet softwares and
hence simplify the form creation process, especially for longer forms. Dive into details [here](http://xlsform.org/)<br>
Both ODK Build and Kobo Build create forms in the same format, XForms or XLSForms, hence ultimately the set of features both forms can
theoretically provide is identical. The onlydistinction comes in because the form builders are designed differently, hence adding some
features may be easier in ODK Build as compared to Kobo Build or vice-versa. Google Forms, in contrast are a proprietary format,
that can only be created using a web browser and [forms.google.com](https://forms.google.com/)<br>
Though this discussion is limited to the free tools available, since ODK is a free and open source software, multiple other tools,
have branched out from the ODK project that also rely on XForms/XLSForms. You can see some of them [here](http://xlsform.org/en/#tools-that-support-xlsforms)

|![XForms/XLSForms](/img/posts/2020-08-06-data-coll-choices-survey-form-creation/0_xform_xlsform.jpg)|
|:--:|
| 0. creating an XForm & XLSForm |


# 1. An overview of the features of Form Builders
software that helps create surveys falls under the category of form builders. Listed here are some core features, each of which is explained in
further detail in sections below.
- **Desktop/Web** : Some platforms have dedicated desktop software for form creation, this enables users to create forms offline, whereas others
simply use your web browser for the same, which requires an active internet connection and signing up for a user account.
- **Question types**: this include the basics, from text and numeric input to more advanced such as barcode input or image/audio input.
- **Metadata questions**: such as start, end times, userid, deviceid, etc fall under metadata questions.
- **Form logic**: is a set of rules for whether or not a question should be displayed based on responses to previous questions.
- **Response validation**: is the application of custom rules to each question being asked to ensure the response is a valid response.
- **Question grouping**: this allows a set of questions to be treated uniformly by other features, such as form logic or form styling.
- **Repeat questions**: is a feature that allows certain questions to be repeated any number of times, depending on answers given at the
time of form submission.
- **Dynamic calculations**: this feature allows values to be used in questions that are dynamically calculated during form entry,
based on responses to previous form questions.
- **Survey templates**: is another feature that some platforms provide, where you don't start from scratch and can simply use externally
benchmarked and validated surveys. In the absence of fully ready-to-use survey templates, tools such as Kobo Form Builder offers a
substitute feature , 'question libraries'.
- **Form styling**: this feature allows forms to be styled in different ways depending on data collector's preferences.
- **Multi-lingual support**: this feature allows the same form to be created in multiple language formats for different audiences.

*tables in this document contain either one of the following four
- ✅ : indicates the feature is possible, using the UI ,
- ❌ : this indicates the feature isn't possible at all
- ~  : the third indicates the feature is possible, but only with workarounds, or knowledge of syntax, which can be non-intuitive at times,
- limited : the last simply indicates a limited subset of the features is possible.<br>

*one might need to scroll left-right to view tables in this post on mobile devices*

|feature                                       | ODK | Kobo | Google Forms |
|:--:|:--:|:--:|:--:|
| desktop tool (offline) form creation         | ✅ | ❌ | ❌ |
| web tool for form creation                   | ✅ | ✅ | ✅ |
| question types                               | 20+ | 20+ | 10 |
| metadata questions [1]                       |  8  |  9  |  1 |
| form (skip) logic [2]                        | ✅ | ✅ | limited |
| response validation                          | ✅ | ✅ | ✅ |
| question grouping                            | ✅ | ✅ | ✅ |
| repeat questions                             | ✅ | ✅ | ❌ |
| dynamic calculations                         | ✅ | ✅ | ❌ |
| survey templates (ready-to-use )             | ❌ | ❌ | ❌ |
| question libraries / build-your-own-template | ❌ | ✅ | ✅ |
| form styling                                 | limited | limited | ❌ |
| multi-lingual forms                          | ✅ | ✅ | ~ |

[1] the audit metadata question (which examines how an enumerator uses a form) isn't available with ODK Build<br>
[2] Google forms only offers form logic based on responses to 'select one'/'select multiple' questions
In comparison, ODK and Kobo allow form logic based on responses to text and numeric questions as well.<br>

## 1a. Desktop and Web
Here's a preview of what building a form in each of the builders looks like. ODK Build pictured here is the desktop tool,
(a similar web tool is also available), whereas Kobo and Google Forms can only be created using a web browser.
Purely from a user experience perspective Google Forms has an advantage, you simply have to type in your question and it
auto-detects the type of question, whether *text*, *numeric*, *date* or *multiple choice*.
Kobo and ODK Build are also fairly simple to use, offering intuitive user interfaces. Kobo hides away some
of the more technical terms under *settings* for each question, and ODK Build has a panel on the right for each questions' settings.
Both UIs have drag and drop functionality as well, to rearrange questions.

Moreover, if one wishes to avoid form builders altogether and build survey forms (XLSForms) in Microsoft Excel, as I enjoy,
that is possible too, if one gains familiarity with the formats there are definite advantages to building XLSForms in Excel,
though for beginners, using a Form Builder is an easy way to get started.

{% include slider.html selector="form_builders_1" %}

## 1b. Question types and Metadata questions
**Fundamental question (widget) types** in ODK and Kobo Build include text, integer, decimal, multiple choice(select_one, select_multiple),
date, time, date+time, geo(point/trace/shape), media (image/audio/video), files, note, acknowledge (to a prompt),
barcode/QR Code inputs, and calculate. Of these google forms has the functionality for all except geo(point/trace/shape), media,
acknowledge (prompts) , barcodes/QR codes, and calculate (dynamic variables)

| question types                          |    ODK Build    |    Kobo Build    |    Google Forms    |
|:--:|:--:|:--:|:--:|
| text                                    | ✅ | ✅ | ✅ |
| integer, decimal                        | ✅ | ✅ | ✅ |
| select_one, select_multiple (multiple-choice)            | ✅ | ✅ | ✅ |
| date, time                              | ✅ | ✅ | ✅ |
| geopoint, geotrace, geoshape            | ✅ | ✅ | ❌ |
| image, audio, video [1]                 | ✅ | ✅ | ~  |
| file                                    | ✅ | ✅ | ✅ |
| note                                    | ✅ | ✅ | ✅ |
| acknowledge (a prompt) [2]              | ✅ | ✅ | ~  |
| barcode/ QR Code                        | ✅ | ✅ | ❌ |
| calculate (dynamic variable)            | ✅ | ✅ | ❌ |

[1] google forms doesn't allow media(image/audio/video) entries directly, but it can be facilitated indirectly by using the 'file' input
this is less than ideal however especially if a form has multiple media entries, an enumerator must take these photos/audio/video separately
and then remember filenames and locations for entry later on.<br>
[2] similarly acknowledge (to a prompt) is also something google forms doesn't directly allow, but this can be implemented using a simple
multiple choice question with select_one input.<br>

**metadata questions** can be useful for instance to identity submissions based on the enumerator's id or a device id.
ODK Build and Kobo both offer `start time`, `end time`, `date`, `device id`, `username`, `simserial`, `subscriberid`, `phonenumber` as metadata fields,
whereas Kobo also offers the audit metadata field, in ODK Build this isn't available. Google Forms doesn't offer any metadata fields.

## 1c. Composite questions, Groups and Repeats
While the fundamental question types have been listed above, form builders also offer **composite question types**.
ODK Build still lacks in options for creating multiple composite question types, hence if you need some of these,
you could consider Kobo Build/Google forms or building forms from scratch in XLSForms format.

Examples of some of these composite questions are listed below. When combining several questions ODK & Kobo Build use groups.
**groups** are simply several questions under one name, using which similar rules can be applied to an entire group at once.

| composite question types                          |    ODK Build    |    Kobo Build    |    Google Forms    |  |
|:--:|:--:|:--:|:--:|
| range                         [1]       | ~  | ✅ | ❌ |![range](/img/posts/2020-08-06-data-coll-choices-survey-form-creation/1_3_range.jpg)|
| ranking                       [2]       | ~  | ✅ | ✅ |![ranking](/img/posts/2020-08-06-data-coll-choices-survey-form-creation/1_3_ranking.jpg )|
| multiple choice grid / rating [3]       | ~  | ✅ | ✅ |![multiple choice grid](/img/posts/2020-08-06-data-coll-choices-survey-form-creation/1_3_mcgrid.jpg)|
| checkbox grid                 [4]       | ~  | ~  | ✅ |![checkbox grid](/img/posts/2020-08-06-data-coll-choices-survey-form-creation/1_3_checkboxgrid.jpg)|
| table/matrix of questions     [5]       | ~  | ✅ | ✅ |![question matrix](/img/posts/2020-08-06-data-coll-choices-survey-form-creation/1_3_matrix.jpg)|
| cascading selects             [6]       | ~  | ✅ | ❌ |![cascading select](/img/posts/2020-08-06-data-coll-choices-survey-form-creation/1_3_cascading.jpg)|

[1] range is simply an **integer** input with restrictions on the range of integers<br>
[2] ranking is simply a **group of select_one** questions that use one list of choices, with the rule applied that each choice is
eliminated from the choice list after it has been chosen once.<br>
[3] **multiple Choice Grid** (Google Forms) **OR** **rating** (Kobo toolbox) both present respondents with a list of questions,
each of which has a 'select one' response, e.g. (yes/no; good/average/bad).
This cab be done in ODK Build with a **group of select_one** questions where each question has the same list of choices<br>
[4] **checkbox grid** questions in Google Forms, can be replicated in Kobo Build or ODK Build as **groups of select_multiple** questions.<br>
[5] tabular/matrix questions are a **group of groups of questions** where each question has a group of questions within.<br>
[6] cascading selects allows the list of choices of one question to be filtered based on the input to the previous question. Hence it is
similar to a sequence of **select_one** questions<br>

**repeats** are another indispensable feature of XForms/XLSForms. They allow an enumerator to flexibly repeat a group of questions any number
of times as the scenario demands. For instance if a set of questions (height, weight, BMI) needs to be inputted for all children in a classroom,
then repeats allow for the height and weight questions to be repeated as many times as there are children in the classroom, without knowing
this number in advance.

## 1d. Response Validation and Form Logic
**response validation** can be applied for text, numeric, and select_multiple questions. With numbers, response validation can be based on
numeric tests, for example the response captured should be less than or greater than a given fixed value, or another value captured by the form. <br>
With text, response validation could be used to limit length of a text string, or check if the string is equal to another string or contains a substring.
With select_multiple, response validation could be used to limit the combination of choices. Here's a comparison of the three tools.

| validation type                               |    ODK Build    |    Kobo Build    |    Google Forms    |
|:--:|:--:|:--:|:--:|
| numeric - less than / greater than / between  | ✅ | ✅ | ✅ |
| numeric - equal to / not equal to       [1]   | ~  | ✅ | ✅ |
| numeric - not between                         | ~  | ~  | ✅ |
| numeric - compare with dynamic variable [2]   | ✅ | ✅ | ❌ |
| text - length of string                       | ✅ | ~  | ✅ |
| text - equal to / not equal to 'pattern'[3]   | ~  | ✅ | ~  |
| text - flexible pattern matching        [4]   | ~  | ~  | ~  |
| select_multiple - prevent certain combinations| ~  | ~  | ❌ |

[1] within ODK Build equal to or not equal to are not available as direct options, but one can implement a workaround using 'range' <br>
[2] this is an important validation field, lacking in Google Forms , but available in both ODK and Kobo with syntax knowledge.
It allows comparing of an inputted numeric value with a previously entered number, for instance, if a respondent indicates their age
is 10 years old , one can validate the input on the `school level` field to ensure grade 6 is not selected.<br>
[3] a text string can be checked to see if it is equal to a particular known text entry, this is useful for quiz forms, to check
correct answers.<br>
[4] Regular Expressions is a form of syntax that can be used to flexibly validate any possible text string combination.
For instance regex is used to check that a valid e-mail id is entered, or a valid phone number, or password.<br>

**form logic** is the core reason why the XForms/XLSForms format, used by both ODK and Kobo are so indispensable. It sets apart
these tools from simpler survey form builders such as Google forms. If the survey you have in mind is more complex in logic
you will likely need to choose ODK/Kobo over Google forms. Form logic offers
the possibility of applying conditions which may result in a True or False answer based on responses to previously inputted
questions. The syntax for form logic rules largely overlaps with the syntax for response validation.  Hence all the examples shown
above for response validation can be used as a logical rule to decide whether or not a question, or group of questions should be
displayed.

## 1e. Dynamic calculations
ODK Build and Kobo Build allow for dynamically calculated fields which allow use of these inputted variables to change the values
or logic of subsequent parts of the form. Google Forms presently doesn't have this feature.

|![calculate](/img/posts/2020-08-06-data-coll-choices-survey-form-creation/1_6_calculation.gif)|
|:--:|
| An example "calculate" question (Source: Kobo Toolbox) |

## 1f. Survey templates
None of the three choices for Form Builders offer ready-to-use templates, but each of them offer the feature to create your own
library of questions or templates, which can later be imported and reused.

## 1g. Form styling
If Survey Forms are made in XLSForm format, using spreadsheet software then styling questions, their text (font, color, positioning)
is possible.

## 1h. Multi-lingual forms
ODK Build and Kobo Build offer the possibility of forms simultaneously made in multiple languages. An enumerator can hence switch
a form from one language to another as convenient. Google Forms does not offer any features for switching the language of a form.
A workaround would be to display both languages simultaneously, however, this can make the forms appear long and cumbersome.

|![calculate](/img/posts/2020-08-06-data-coll-choices-survey-form-creation/1_9_multi-lingual.jpg)|
|:--:|
| Setting a form to be multi-lingual in ODK Build |
