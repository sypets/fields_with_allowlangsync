# fields_with_allowlangsync

This is a convenience extension to easily test TYPO3 allowLanguageSynchronization.

It sets the allowLanguageSynchronization behaviour for these fields:

* tt_content.assets (used in "Text & Media")
* tt_content.image (used in "Text & Images")
* tt_content.media (used in "Uploads")

## Reproduce bug

It can be used to reproduce the TYPO3 core bug https://forge.typo3.org/issues/88980

as described in the issue (slightly modified):

Steps to reproduce:
1. Create a new ("Text & Media") record, set language to [All]
2. Save
3. Add immage as fal relation (tab "media", field "assets")
4. Save
5. Set language to Default [0]
6. Save
7. Translate by creating a translation of the page and pressing "Translate" button, use connected mode

Now the translate dialog is stuck in "Processing" (Spinner).

However, there is a log message in the logs:

Tue, 17 Dec 2024 17:37:07 +0000 [CRITICAL] request="1b1e5f75504ef" component="TYPO3.CMS.Core.Error.DebugExceptionHandler":
Core: Exception handler (WEB: BE): RuntimeException, code #1486233164,
file /var/www/t3coredev14/typo3/sysext/core/Classes/DataHandling/Localization/DataMapProcessor.php,
line 607:
Child record was not processed- RuntimeException:
Child record was not processed, in file /var/www/t3coredev14/typo3/sysext/core/Classes/DataHandling/Localization/DataMapProcessor.php:607

When the page is reloaded, there are 2 copies of the file relation in the record
of the original language.
