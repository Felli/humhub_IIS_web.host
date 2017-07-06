# humhub_IIS_web.host
=====================

Know issues:
------------
- Only tested on 15 dev installations  and 3 production sites
- So, it needs more testing ;)



Rewrite rules *ONLY*:
-------------------------------------

The web.host have may settings and adjustments  so it may not work for everyone.

In case you just what the IIS rewrite rules:

```<rules>
                <clear />
			
				<!--
				Remove/force the WWW from the URL.
				Requires IIS Rewrite module http://learn.iis.net/page.aspx/460/using-the-url-rewrite-module/
				Configuration lifted from http://nayyeri.net/remove-www-prefix-from-urls-with-url-rewrite-module-for-iis-7-0

				NOTE* You need to install the IIS URL Rewriting extension (Install via the Web Platform Installer)
				http://www.microsoft.com/web/downloads/platform.aspx

				** Important Note
				using a non-www version of a webpage will set cookies for the whole domain making cookieless domains
				(eg. fast cdn-like access of static resources like css, js and images) impossible.

				# IMPORTANT: THERE ARE TWO RULES LISTED. NEVER USE BOTH RULES AT THE SAME TIME!
							===========================
				-->
							
							<!--
							<rule name="Remove WWW" stopProcessing="true">
								<match url="^(.*)$" />
								<conditions>
									<add input="{HTTP_HOST}" pattern="^(www\.)(.*)$" />
								</conditions>
								<action type="Redirect" url="http://example.com{PATH_INFO}" redirectType="Permanent" />
							</rule>
							-->
							
							<!--
							<rule name="Force WWW" stopProcessing="true">
								<match url=".*" />
								<conditions>
									<add input="{HTTP_HOST}" pattern="^example.com$" />
								</conditions>
								<action type="Redirect" url="http://www.example.com/{R:0}" redirectType="Permanent" />
							</rule>
							-->
				
				<!--
				### Built-in filename-based cache busting

				In a managed language such as .net you should really be using the internal bundler for css + js
				or getCassette or  similar.

				If you're not using the build script to manage your filename version revving,
				you might want to consider enabling this, which will route requests for
				/css/style.20110203.css to /css/style.css

				To understand why this is important and a better idea than all.css?v1231,
				read: http://madskristensen.net/post/cache-busting-in-aspnet
				-->

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
                    <!-- <action type="AbortRequest" /> -->
					<!-- <action type="CustomResponse" statusCode="403" statusReason="Forbidden" statusDescription="Forbidden" /> -->
                </rule>


				<rule name="block dev files bak|bat|config|sql|fla|md|psd|ini|log|sh|inc|swp|dist|composer.json|composer.lock|grunt.-js" enabled="true" patternSyntax="ECMAScript" stopProcessing="true">
					<match url="(\.(bak|bat|config|sql|fla|md|psd|ini|log|sh|inc|swp|dist|ai)|(\/c|^c)omposer\.json|(\/c|^c)omposer.lock|(\/g|^g)runt\.js)$" ignoreCase="true" negate="false" />
					<conditions logicalGrouping="MatchAll">
					</conditions>
					<action type="Redirect" url="404.php?r=ext" />
					<!-- <action type="AbortRequest" /> -->
					<!-- <action type="CustomResponse" statusCode="403" statusReason="Forbidden" statusDescription="Forbidden" /> -->
				</rule>

				<rule name="block files and folderd Yii and Node specific" enabled="true" patternSyntax="ECMAScript" stopProcessing="true">
					<match url="(^protected\/|^node_modules\/|^protected\/yii|^uploads\/file\/)" ignoreCase="false" />
					<action type="Redirect" url="404.php?r=protected" />
					<!-- <action type="AbortRequest" /> -->
					<!-- <action type="CustomResponse" statusCode="403" statusReason="Forbidden" statusDescription="Forbidden" /> -->
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
						<!-- <add input="{URL}" pattern="^\/*.png$" ignoreCase="false" negate="true" /> -->
						</conditions>
					<action type="Rewrite" url="index.php" />
				</rule>


            </rules>```
