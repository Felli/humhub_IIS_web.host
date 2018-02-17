# humhub_IIS_web.host
=====================

Know issues:
------------
- Only tested on 15 dev installations  and 3 production sites
- So, it needs more testing ;)


*Web.host*:
-------------------------------------
The web.host have all settings and adjustments we have needed but this may be too much for everyone, we recommend to:

 *test it in a development environment and remove all unnecessary parts.*

In case you just what the IIS rewrite rules see below




Rewrite rules *ONLY*:
-------------------------------------

The web.host have may settings and adjustments  so it may not work for everyone.

In case you just what the IIS rewrite rules:
Rewrite rules *ONLY*:
-------------------------------------

The web.host have may settings and adjustments  so it may not work for everyone.

In case you just what the IIS rewrite rules:

```
<rules>
   <clear />
	<rule name="https EXCEPTION for SSL cerficicate check ( excelude folder and all /.well-known/)" enabled="true" stopProcessing="true">
		<match url=".well-known/(.*)" />
		<conditions logicalGrouping="MatchAll" trackAllCaptures="false">
		</conditions>
		<action type="None" />
	</rule>

	<rule name="Redirect to HTTPS" enabled="true" stopProcessing="true">
		<match url="(.*)" />
		<conditions>
			<add input="{HTTPS}" pattern="^OFF$" />
		</conditions>
		<action type="Redirect" url="https://{HTTP_HOST}/{R:1}" redirectType="SeeOther" />
	</rule>

	<rule name="Deny files and folderd starting with . but allow folder .well-known/*" enabled="true" patternSyntax="ECMAScript" stopProcessing="true">
		<match url="(^\.|\/\.(?!well-known))" ignoreCase="true" negate="false" />
		<conditions logicalGrouping="MatchAll">
		</conditions>
		<action type="Redirect" url="404.php?r=dot" />
	</rule>


	<rule name="block dev files bak|bat|config|sql|fla|md|psd|ini|log|sh|inc|swp|dist|composer.json|composer.lock|grunt.-js" enabled="true" patternSyntax="ECMAScript" stopProcessing="true">
		<match url="(\.(bak|bat|config|sql|fla|md|psd|ini|log|sh|inc|swp|dist|ai)|(\/c|^c)omposer\.json|(\/c|^c)omposer.lock|(\/g|^g)runt\.js)$" ignoreCase="true" negate="false" />
		<conditions logicalGrouping="MatchAll">
		</conditions>
		<action type="Redirect" url="404.php?r=ext" />
	</rule>

	<rule name="block files and folderd Yii and Node specific" enabled="true" patternSyntax="ECMAScript" stopProcessing="true">
		<match url="(^protected\/|^node_modules\/|^protected\/yii|^uploads\/file\/)" ignoreCase="false" />
		<action type="Redirect" url="404.php?r=protected" />
	</rule>


	<rule name="Allow images form /upload/logo_image/* and upload/profile_image\/*" enabled="true" patternSyntax="ECMAScript" stopProcessing="true">
		<match url="(^uploads\/logo_image\/*|^uploads\/profile_image\/*)" ignoreCase="false" />
		<action type="None" />
	</rule>


	<rule name="Yii2 enablePrettyUrl" enabled="true" stopProcessing="true">
		<match url="^(.*)$" ignoreCase="false" />
		<conditions>
			<add input="{REQUEST_FILENAME}" matchType="IsFile" ignoreCase="false" negate="true" />
			<add input="{REQUEST_FILENAME}" matchType="IsDirectory" ignoreCase="false" negate="true" />
			</conditions>
		<action type="Rewrite" url="index.php" />
	</rule>


</rules>
```

Please, note:
-------------
 - 1st rule is ONLY when using FREE SSL from https://certifytheweb.com/
 - 2nd is ONLY need if you want to force HTTP*S*



# ANY HELP, COMMENT, QUESTION, JOKE OR ISSUE is more than welcome!!!
