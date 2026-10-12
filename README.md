# scorm-requirements-exercises

Exercise catalog for the SCORM packages of the requirements engineering course. The packages read the exercises directly from the `main` branch of this repository, so an exercise is published by pushing it here. The packages do not need to be rebuilt or uploaded again to Moodle.

| Folder | Used by | Exercise |
|---|---|---|
| [`requirements-reviews`](requirements-reviews) | [scorm-requirements-reviews](https://github.com/GRISE-UPM/scorm-requirements-reviews) | Find the quality defects of a requirements specification |
| [`requirements-testing`](requirements-testing) | [scorm-requirements-testing](https://github.com/GRISE-UPM/scorm-requirements-testing) | Identify the testing requirements of a document and classify them as actions, rules, or states |

## Layout of an exercise folder

```
requirements-reviews/
├── contents.xml        catalog: lists the exercise files
└── exercises/
    ├── SRS-elevator.xml
    └── ...
```

`contents.xml` only lists the files. The title of an exercise is read from the exercise file, so it is kept in one place only:

```xml
<contents>
    <document><file>SRS-elevator.xml</file></document>
    <document><file>SRS-acme-library.xml</file></document>
</contents>
```

An exercise that is listed but is not in the repository, or has no title, is skipped by the packages. If no exercise is available, the student sees a message on the start page. A `<title>` inside `contents.xml` is ignored.

## Exercises of the requirements review

Each file has the title, optional instructions, and the text of the specification in Markdown. The seeded defects are wrapped in `<defect>` tags inside the text:

```xml
<document>
    <title>SRS: ACME elevator FAMILY model</title>
    <instructions></instructions>
    <text>
# 1 Introduction
...
FUN(02) <defect type="Conforming" weight="1" comment="Section 1.5 requires the verb “shall”...">The system should ignore a floor request.</defect>
    </text>
</document>
```

- `type` is the characteristic of the individual requirements that the defect violates. The nine characteristics are defined in ISO/IEC/IEEE 29148:2018: Necessary, Appropriate, Unambiguous, Complete, Singular, Feasible, Verifiable, Correct, and Conforming.
- `weight` is the score of the defect. It may be a decimal, for example `0.5`, if a defect is worth less or more than the others.
- `comment` explains the defect to the student when the solution is compared.
- The `<instructions>` text, if it is not empty, is shown with a "View instructions" button.
- Headings use `#`, `##`, and `###`, and lists use `- `. Requirements are written one per line.

The review of a specification should leave no defects other than the seeded ones.

### Review status of the specifications

`contents.xml` also records which specifications have not been reviewed yet. Those are listed in a `<notReviewed>` element, which the packages ignore, so they are not offered to the students. A specification is published by moving its `<file>` from `<notReviewed>` to a `<document>`.

```xml
<contents>
    <document><file>SRS-elevator.xml</file></document>
    <notReviewed>
        <file>SRS-microwave.xml</file>
    </notReviewed>
</contents>
```

### Working with Word files

The specifications can be reviewed in Word, with each defect shown as a comment ("Defect seed"). The generated `.docx` files are working files kept in `tmp` and are not part of the catalog.

## Exercises of the requirements testing

Each file is a document with requirements and subrequirements. `type` says how the student should classify the line:

```xml
<exercises>
    <title>Exercise 1</title>
    <exercise id="[RF 44]">
        <title>When the user is able to 'take' a notification</title>
        <exerciseType>0</exerciseType>
        <type>3</type>
        <subrequirements>
            <subrequirement>
                <title>do the action and mark the notification as 'finished'.</title>
                <type>1</type>
            </subrequirement>
        </subrequirements>
    </exercise>
</exercises>
```

- `type` is `1` for an action, `2` for a rule, `3` for a state, and `0` for a line that is not a testing requirement.
- The `<title>` that is a direct child of `<exercises>` is the title of the exercise in the list shown to the student.

## Publishing an exercise

1. Add the file to `exercises/` in the folder of the app.
2. List it in `contents.xml`.
3. Push to the `main` branch. The packages read the raw files from GitHub, so the exercise is available as soon as the push is done.
