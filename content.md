<!--
Version 0.1
-->
<!--
VERSION TODO #1: Never delete the version comment from your content.md file.
This specifies the schema version of the document so that it may be parsed
correctly when it is imported into Lab on Demand. Once you understand the
importance of the version comment, you may delete this comment (but not the
version comment before it).
-->

<!--
TOC TODO #1: As you rename First Exercise and First Task below, change their
names in the table of contents and update their anchors (name in lowercase with
dashes for spaces). As you add additional exercises and tasks, add them to the
table of contents as well, observing the following rules:
1. Always prefix TOC entries by "1.". Markdown will assign an appropriate
   sequential number to the entry.
2. When indenting to create sub-entries (which will be numbered with a,b,... or
   i,ii,...) always indent 4 spaces (GitHub only requires 2, however 4 is the
   markdown standard).
3. For hyperlinking to work, TOC entries should be entered in the following
   format:
   [Exercise or task name](#exercise-or-task-name-with-dashes-for-spaces)
4. The TOC is currently not used by the content.md parser. It is simply a nice
   ways to get fast access to various tasks and exercises. Future versions of
   the parser may leverage the TOC to define exercise/task positioning as a
   content.md file is imported.

If you do not want a table of contents for your markdown document, remove this
comment as well as the table of contents below this comment.

Once you understand how a table of contents works, remove this comment.
-->
#  Table of Contents
1. [First Exercise](#first-exercise)
    1. [First Task](#first-task)

* * *

# First Exercise

<!--
EXERCISE TODO #1: Replace "First Exercise" in the heading above with the name
of your new exercise.
-->

<!--
EXERCISE TODO #2: Set the IntroductionUri and CompletionUri values in the quoted
properties below. Both IntroductionUri and CompletionUri may be relative (within
GitHub) or absolute uris. Remove any values that you don't need, removing the
entire quote if you don't need any of the values. Then delete this comment.
-->

>LODSProperties
>* IntroductionUri = 
>* CompletionUri = 

## INTRODUCTION MESSAGE

<!--
EXERCISE TODO #3: Replace this comment with the introduction message for this
exercise. This message can span multiple paragraphs if necessary.
-->

## COMPLETION MESSAGE

<!--
EXERCISE TODO #4: Replace this comment with a completion message for this
exercise, or delete the COMPLETION MESSAGE heading and this comment if you do
not need a completion message.
-->

<hr>

### New Task

<!--
TASK TODO #1: Replace "New Task" in the heading above with the name of your
new task and replace this comment with the task instruction message. For the
best results, try to keep this message short (200 characters or less) and to a
single paragraph.
-->

#### :warning: ALERT

<!--
TASK TODO #2: Replace this comment with any warning text that you want
displayed when a student advances to this task. If you don't have any warning
text to display for this task, delete this comment and the ALERT heading
before it.
--> 

#### :bulb: KNOWLEDGE

<!--
TASK TODO #3: Replace this comment with any knowledge text that you want
displayed when a student clicks on the Knowledge button. Knowledge text must
not be required for students to complete a task. It is used to provide students
with additional details, hints, or alternative ways to perform a task. If you
do not have any knowledge text to display for this task, delete this comment
and the KNOWLEDGE heading before it.
--> 

#### :camera: SCREENSHOT

<!--
TASK TODO #4: In the quoted properties below, set the Uri property to the uri of
a screenshot you want to link to this task. This can be a relative (within
GitHub) or absolute uri. Then set the ShowAutomatically property to one of the
following:
- No, if you only want the screenshot to appear when the student clicks on the
camera button;
- Once, if you want the screenshot to appear automatically the first time the
student advances to this task; or
- EveryTime, if you want the screenshot to appear automatically every time the
student advances or returns to this task.

Once you have set the screenshot properties, delete this entire comment.

If you do not have a screenshot to associate with this task, delete the
SCREENSHOT heading above this comment as well as this comment and the quote
below it.
-->  
>LODSProperties
>* Uri = 
>* ShowAutomatically = No|Once|EveryTime

#### :movie_camera: VIDEO

<!--
TASK TODO #5: In the quoted properties below, set the Uri property to the uri of
a video you want to link to this task. This can be a relative (withing GitHub)
or absolute uri. Then set the ShowAutomatically property to one of the
following:
- No, if you only want the video to appear when the student clicks on the video
camera button;
- Once, if you want the video to appear automatically the first time the
student advances to this task; or
- EveryTime, if you want the video to appear automatically every time the
student advances or returns to this task.

If you want the video to appear in a separate dialog, set ShowInDialog to true;
otherwise, set it to false. 

Once you have set the video properties, delete this entire comment.

If you do not have a video to associate with this task, delete the VIDEO
heading above this comment as well as this comment and the quote below it.
-->
>LODSProperties
>* Uri = 
>* ShowAutomatically = No|Once|EveryTime
>* ShowInDialog = true|false

#### :calling: COMMAND TypeText|PowerShell|PowerShellWithUI|Shell|ShellWithUI

<!--
TASK TODO #6: In the heading above, choose the command type (TypeText,
PowerShell, PowerShellWithUI, Shell, ShellWithUI) appropriate for this command
and remove the others. If the command type is PowerShell or PowerShellWithUI,
leave the "PowerShell" language specifier that is next to the code block
opening enclosure; otherwise, remove the language specifier and simply leave
the code block opening enclosure with nothing after it. Then, enter the
appropriate command text in the code block below.

Once you have set up the command type and code block properly, delete this
entire comment.

If you do not have a command to associate with this task, delete the COMMAND
heading above this comment as well as this comment and the code block below it.
-->
```PowerShell
# Replace this line with the PowerShell command, shell command, or text to type.
# If this command is anything other than PowerShell or PowerShellWithUI, remove
# the "PowerShell" label at the beginning of this code block.
```

#### :computer: ACTIONS

<!--
TASK TODO #7: In the quoted properties below, set the VM property to one of the
following:
- NoAction, if you don't want to change the active VM in the lab;
- VNName, if you want to select a different VM in the lab as the active VM
(Note that you must enter the name of the VM in place of the "VMName" string in
order for this to work).

Then set the FloppyDrive property to one of the following:
- NoAction, if you don't want to change the state of the virtual floppy drive
in the active VM;
- FloppyName, if you want to insert a different floppy disk into the virtual
floppy drive in the active VM (Note that you must enter the name of the floppy
disk in place of the "FloppyName" string in order for this to work);
- Eject, if you want to eject the floppy disk in the virtual floppy drive in
the active VM.

Then set the DvdDrive property to one of the following:
- NoAction, if you don't want to change the state of the virtual DVD drive
in the active VM;
- DvdName, if you want to insert a different DVD disk into the virtual DVD
drive in the active VM (Note that you must enter the name of the DVD disk
in place of the "DvdName" string in order for this to work);
- Eject, if you want to eject the DVD disk in the virtual DVD drive in the
active VM.

Once you have configured the actions for the task, delete this entire comment.

If you do not want to take any of these actions with this task, delete the
ACTIONS heading above this comment as well as this comment and the quote below
it.
-->
>LODSProperties
>* VM = NoAction|VMName
>* FloppyDrive = NoAction|FloppyName|Eject
>* DvdDrive = NoAction|DvdName|Eject
>* DvdDrive = NoAction|DvdName|Eject

<!--
NEW TASK TODO #1: If you want to add another task, copy and paste the contents of
the task template you want to use over this comment. You can find the task
templates here:
https://github.com/LearnOnDemandSystems/idl-md/blob/master/templates
-->

<!--
NEW EXERCISE TODO #1: If you want to add another exercise, copy and paste the
contents of the exercise template you want to use over this comment. You can find
the exercise templates here:
https://github.com/LearnOnDemandSystems/idl-md/blob/master/templates
-->