# body-to-category

Server-side injection of tag or warning message strings directly in the email body is not only a technically unfortunate implementation but has numerous drawbacks for users (e.g. it is not automatically stripped from replies)

This Outlook VBA macro repairs the email body by removing such strings from from the email body and sets a more appropriate [custom category](https://support.office.com/en-us/article/Create-and-assign-color-categories-a1fde97e-15e1-4179-a1a0-8a91ef89b8dc) instead.

This macro achieves the following:
- it removes a specific string from the email body of any email received in the last two days or any newly incoming email
- and sets a custom category instead so emails are still marked
- the string is currently set to "External email: use caution" with surrounding asterisks (line 23)

## Screenshots

Without macro:
![Without macro](https://raw.githubusercontent.com/jerogee/els-tag-repair/master/img/ss_without.png)

With macro:
![With macro](https://raw.githubusercontent.com/jerogee/els-tag-repair/master/img/ss_with.png)


## Installation

The easiest way to install this macro is:
* Download the macro file [BodyToCategory.bas](https://github.com/jerogee/body-to-category/raw/master/BodyToCategory.bas)
* In Outlook, press `Alt` + `F11` to bring up the `VBA` environment.
* In the Project pane, Right-click on `Project1`, select `Import File...`, open `BodyToCategory.bas`
* Under `Project1`, double-click on the built-in `ThisOutlookSession` module to open it.
* Copy & paste the following two calls, for triggering the macro upon receiving new email and upon startup:
```VB.net
Private Sub Application_NewMailEx(ByVal EntryIDCollection As String)
    StripCollection EntryIDCollection
End Sub

Private Sub Application_Startup()
    StripInbox do_all_messages:=False
End Sub
```
* Select `File | Save`, and close the editor.
* To be able to run the macro, set the security settings appropriately: In Outlook 2007 and higher, the macro security settings are in `Options | Trust Center | Trust Center Settings... | Macro Settings` dialog. Set macro security to `Notifications for all macros`. 
* Upon (re)starting Outlook, outlook will ask whether to enable the macro or not.


## Notes

If you need to customize the macro for a specific string, please change line 53 of BodyToCategory.bas. You then may also need to change the regular expressions on lines 31 and 35.

Finally, do realise that enabling macros, even in a restrictive way, may make Outlook more vulnerable. If possible, please [self-sign](https://support.office.com/en-gb/article/Digitally-sign-a-macro-project-6e5de679-01d4-4387-85e0-92e3e9a49483) the macro and set the Trust Center settings to only accept signed macros.
