---
layout: page
title: User Guide
---

AddressBook Level 3 (AB3) is a **desktop app for managing contacts, optimized for use via a Command Line Interface** (CLI) while still having the benefits of a Graphical User Interface (GUI). If you can type fast, AB3 can get your contact management tasks done faster than traditional GUI apps.

* Table of Contents
{:toc}

--------------------------------------------------------------------------------------------------------------------

## Quick start

1. Ensure you have Java `17` or above installed in your Computer.

1. Download the latest `.jar` file from [here](https://github.com/se-edu/addressbook-level3/releases).

1. Copy the file to the folder you want to use as the _home folder_ for your AddressBook.

1. Open a command terminal, `cd` into the folder you put the jar file in, and use the `java -jar addressbook.jar` command to run the application.<br>
   A GUI similar to the below should appear in a few seconds. Note how the app contains some sample data.<br>
   ![Ui](images/Ui.png)

1. Type the command in the command box and press Enter to execute it. e.g. typing **`help`** and pressing Enter will open the help window.<br>
   Some example commands you can try:

   * `list` : Lists all contacts.

   * `add n/John Doe p/98765432 e/johnd@example.com a/John street, block 123, #01-01` : Adds a contact named `John Doe` to the Address Book.

   * `delete John Doe` : Deletes the John Doe contact.

   * `clear` : Deletes all contacts.

   * `exit` : Exits the app.

1. Refer to the [Features](#features) below for details of each command.

--------------------------------------------------------------------------------------------------------------------

## Features

<div markdown="block" class="alert alert-info">

**:information_source: Notes about the command format:**<br>

* Words in `UPPER_CASE` are the parameters to be supplied by the user.<br>
  e.g. in `add n/NAME`, `NAME` is a parameter which can be used as `add n/John Doe`.

* Items in square brackets are optional.<br>
  e.g `n/NAME [t/TAG]` can be used as `n/John Doe t/friend` or as `n/John Doe`.

* Items with `…`​ after them can be used multiple times including zero times.<br>
  e.g. `[t/TAG]…​` can be used as ` ` (i.e. 0 times), `t/friend`, `t/friend t/family` etc.

* Parameters can be in any order.<br>
  e.g. if the command specifies `n/NAME p/PHONE_NUMBER`, `p/PHONE_NUMBER n/NAME` is also acceptable.

* Extraneous parameters for commands that do not take in parameters (such as `help`, `list`, `exit` and `clear`) will be ignored.<br>
  e.g. if the command specifies `help 123`, it will be interpreted as `help`.

* If you are using a PDF version of this document, be careful when copying and pasting commands that span multiple lines as space characters surrounding line-breaks may be omitted when copied over to the application.
</div>

### Viewing help : `help`

Shows a message explaning how to access the help page.

![help message](images/helpMessage.png)

Format: `help`


### Adding a person: `add`

Adds a person to the address book. People with same names are allowed.

Format: `add n/NAME p/PHONE_NUMBER [e/EMAIL] [a/ADDRESS] [i/INCOME<none/low/mid/high>] [age/AGE] [t/TAG]…​`

<div markdown="span" class="alert alert-primary">:bulb: **Tip:**
A person can have any number of tags (including 0)
</div>

Examples:
* `add n/John Doe, Alexander p/98765432 e/johnd@example.com a/John street, block 123, #01-01 i/high age/33`
* `add n/Betsy d/o Crowe t/boss e/betsycrowe@example.com a/Jurong West Street p/+651234567 t/golf i/MID age/56`
* `add n/Mary Jane t/client p/+651234567 i/low age/24`

### Listing all persons: `list`

Shows a list of all persons in the address book. The list can be optionally sorted by specified fields.

Format: `list [s/SORT_FIELD] [r/]`

Parameters:
* `s/SORT_FIELD`: The field to sort by. Valid values are `name`, `email`, `income`, or `age`. If not provided, no sorting will be applied.
* `r/`: If present, the sort order will be reversed (descending). If not present, the sort order will be ascending.

Examples:
* `list`: Lists all persons without any sorting.
* `list s/name`: Lists all persons sorted by name in ascending order.
* `list s/email r/`: Lists all persons sorted by email in descending order.
* `list s/income`: Lists all persons sorted by income in ascending order.
* `list s/age r/`: Lists all persons sorted by age in descending order.

Notes:
* If `s/SORT_FIELD` is provided without `r/`, the list will be sorted in ascending order.
* If `r/` is provided without `s/SORT_FIELD`, it will have no effect.

### Editing a person : `edit`

Edits an existing person in the address book.

Format: `edit INDEX [n/NAME] [p/PHONE] [e/EMAIL] [a/ADDRESS] [i/INCOME<none/low/mid/high>] [age/AGE] [t/TAG]…​`

* Edits the person at the specified `INDEX`. The index refers to the index number shown in the displayed person list. The index **must be a positive integer** 1, 2, 3, …​
* At least one of the optional fields must be provided.
* Existing values will be updated to the input values.
* When editing tags, the existing tags of the person will be removed i.e adding of tags is not cumulative.
* You can remove all the person’s tags by typing `t/` without
    specifying any tags after it.

Examples:
*  `edit 1 p/91234567 e/johndoe@example.com` Edits the phone number and email address of the 1st person to be `91234567` and `johndoe@example.com` respectively.
*  `edit 2 n/Betsy Crower t/` Edits the name of the 2nd person to be `Betsy Crower` and clears all existing tags.

### Locating persons by name: `find`

Finds persons whose names contain any of the given keywords. If no exact match is found, the address book displays the names in decreasing order of similarity to search term.

Format: `find KEYWORD [MORE_KEYWORDS]`

* The search is case-insensitive. e.g `hans` will match `Hans`
* The order of the keywords does not matter. e.g. `Hans Bo` will match `Bo Hans`
* Only the name is searched.
* Partial words will be matched e.g. `Han` will match `Hans`
* Persons matching at least one keyword will be returned (i.e. `OR` search).
  e.g. `Hans Bo` will return `Hans Gruber`, `Bo Yang`
* If no exact match is found, the address book displays the names in decreasing order of similarity to search term.

Examples:
* `find John` returns `john`, `John Doe`, and `Johnny`
* `find alex david` returns `Alex Yeoh`, `David Li`<br>
  ![result for 'find alex david'](images/findAlexDavidResult.png)
* `find Ifan` sorts other names by decreasing similarity to `Ifan` (e.g. Irfan, Isolde, Ayush, ...)

### Filtering persons by criteria: `filter`

Filters the displayed list of persons in the address book to include all persons who meet the specified criteria and displays them with index numbers.

Format: `filter p/PHONE e/EMAIL a/ADDRESS t/TAG... i/INCOME_GROUP... age/AGE_CRITERIA...`

Parameters:
* `p/PHONE`: The phone number criteria to filter by. For multiple phone numbers, it checks if any phone number is present.
* `e/EMAIL`: The email criteria to filter by. For multiple emails, it checks if any email is present.
* `a/ADDRESS`: The address criteria to filter by. For multiple addresses, it checks if any address is present.
* `t/TAG...`: The tags to filter by. For multiple tags, it checks if all tags are present.
* `i/INCOME_GROUP...`: The income group criteria to filter by. Valid values are `none`, `low`, `medium`, and `high`.
* `age/AGE_CRITERIA...`: The age criteria to filter by. A valid age criteria can only contain `<`, `>`, or numbers. If it contains `<` or `>`, there must only be a single instance of either of them, and only as the first character. It cannot contain both. If only numbers are given, equality is checked. For multiple age criteria, it checks if all age criteria are satisfied.

**Examples**:
* `filter p/+65 e/example.com a/Clementi t/Inactive i/low age/>20 <60`: Filters the list to include all persons whose phone number contains `+65`, email contains `example.com`, address contains `Clementi`, have the tag `Inactive`, belong to the `low` income group, and are aged between 21 and 59.
* `filter p/987 e/johndoe@example.com a/Street i/medium high age/30`: Filters the list to include all persons whose phone number contains `987`, email is `johndoe@example.com`, address contains `Street`, belong to either the `medium` or `high` income group, and are exactly 30 years old.

**Notes**:
* At least one of `p/PHONE`, `e/EMAIL`, `a/ADDRESS`, `i/INCOME_GROUP`, `age/AGE_CRITERIA` or `t/TAG...` must be provided.
* If a contact does not have a value for the criteria field, it is not displayed.
* Multiple criteria for phone, email, address, tags, income group, and age can be specified, separated by spaces.
* The criteria for phone, email, address, and tags are case-insensitive and can be partial matches.
* The criteria income group only checks for exact matches.

### Viewing, adding and editing notes: `notes` 

Views, adds, edits, or deletes the notes of the person identified by their name or index.

Format: `notes [PARAMETER]`

Parameters:
* View: [view/NAME] or [view/INDEX]
* Add: [add/NAME nt/NOTES] or [add/INDEX nt/NOTES]
* Edit: [edit/NAME] or [edit/INDEX]
* Delete: [del/NAME] or [del/INDEX]

**Examples**:
* `notes view/John Doe` OR `view/1`
* `notes add/John Doe nt/Prefers email contact` OR `add/1 nt/Prefers email contact`
* `notes edit/John Doe` OR `edit/1`
* `notes del/John Doe` OR `del/1`

### Deleting a person : `delete`

Deletes the person with the specified `NAME` or the person at the specified `INDEX`

Format: `delete NAME` or `delete INDEX`

* For deletion by `NAME`:
  * Deletes the person with the specified `NAME`.
  * Only delete if `NAME` is exact match with contact full name.
  * Delete is case-insensitive. e.g `hans` will delete `Hans`.
  * If no exact match, list will be filtered based on given `NAME`.
    AddressBook will then prompt user to use fullname or `INDEX`.
  * If no exact and partial match,
    AddressBook will then prompt user to use another `NAME` or `INDEX`.
  * If more than 1 exact match, list will be filtered based on given `NAME`.
    AddressBook will then prompt user to use `INDEX` instead.

* For deletion by `INDEX`:
  * Deletes the person at the specified INDEX.
  * The index refers to the index number shown in the displayed person list.
  * The index must be a positive integer 1, 2, 3, …​

Examples:
* `delete Alice`: Deletes the person named Alice from the address book.
* `delete 1`: Deletes the first person in the currently displayed list.

### Clearing all entries : `clear`

Clears all entries from the address book.

Format: `clear`

### Command History : `↑` `↓` 
Allows users to quickly access previously entered commands without retyping them using arrow keys.

Format: `↑` or `↓`

- Up Arrow `↑`: Displays the last command you entered. Press multiple times to view older commands in reverse order.
- Down Arrow `↓`: Moves forward through the command history, displaying newer commands. Once you reach the most recent 
command, pressing `↓` again will clear the command box.

Examples:
- `↑` : Retrieve previous command in the command box.
- `↓` : Retrieve next command in the command box.

### Change theme : `F3`

Allows users to toggle between light and dark theme with `F3` key

Format: `F3`

### Exiting the program : `exit`

Exits the program.

Format: `exit`

### Saving the data

AddressBook data are saved in the hard disk automatically after any command that changes the data. There is no need to save manually.

### Editing the data file

AddressBook data are saved automatically as a JSON file `[JAR file location]/data/addressbook.json`. Advanced users are welcome to update data directly by editing that data file.

<div markdown="span" class="alert alert-warning">:exclamation: **Caution:**
If your changes to the data file makes its format invalid, AddressBook will discard all data and start with an empty data file at the next run. Hence, it is recommended to take a backup of the file before editing it.<br>
Furthermore, certain edits can cause the AddressBook to behave in unexpected ways (e.g., if a value entered is outside of the acceptable range). Therefore, edit the data file only if you are confident that you can update it correctly.
</div>

### Archiving data files `[coming in v2.0]`

_Details coming soon ..._

--------------------------------------------------------------------------------------------------------------------

## FAQ

**Q**: How do I transfer my data to another Computer?<br>
**A**: Install the app in the other computer and overwrite the empty data file it creates with the file that contains the data of your previous AddressBook home folder.

--------------------------------------------------------------------------------------------------------------------

## Known issues

1. **When using multiple screens**, if you move the application to a secondary screen, and later switch to using only the primary screen, the GUI will open off-screen. The remedy is to delete the `preferences.json` file created by the application before running the application again.
2. **If you minimize the Help Window** and then run the `help` command (or use the `Help` menu, or the keyboard shortcut `F1`) again, the original Help Window will remain minimized, and no new Help Window will appear. The remedy is to manually restore the minimized Help Window.

--------------------------------------------------------------------------------------------------------------------

## Command summary

Action | Format, Examples
--------|------------------
**Add** | `add n/NAME p/PHONE_NUMBER [e/EMAIL] [a/ADDRESS] [t/TAG]…​` <br> e.g., `add n/James Ho p/22224444 e/jamesho@example.com a/123, Clementi Rd, 1234665 t/friend t/colleague`
**Clear** | `clear`
**Delete** | `delete INDEX` or `delete NAME` <br> e.g., `delete 3` `delete James Ho`
**Edit** | `edit INDEX [n/NAME] [p/PHONE_NUMBER] [e/EMAIL] [a/ADDRESS] [t/TAG]…​`<br> e.g.,`edit 2 n/James Lee e/jameslee@example.com`
**Find** | `find KEYWORD [MORE_KEYWORDS]`<br> e.g., `find James Jake`
**List** | `list`
**Help** | `help`
