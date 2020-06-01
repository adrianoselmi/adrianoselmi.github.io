---
layout: post
title:  "Site Designs: adding a column hidden from new, edit and view forms"
date:   2020-05-31 20:57:09 +0100
category: SharePoint
tag: Site Design
excerpt_separator: <!--more-->
---
<H2>Requirement:</H2>
<p>Using Site Designs, deploy a site column that is hidden in the new, display or edit forms. The column should only be viewable through views and be editable through the Quick edit option in views.</p>
<!--more-->

<hr>

<H2>Background:</H2>
<H5>Image:Setting a column to hidden in a content type</H5>
![Setting a column to hidden in the GUI](/assets/images/HiddenColumn01.gif "Hidden Column")


<p>To create the site design, a template site in SharePoint was utulised to test and fine tune all the columns, content types, lists, and libraries. Using PowerShell to generate the JSON provided most of the information but did not include the above settings for any columns that were set to 'Hidden' in the content type settings.</p>

<H5>PowerShell Snippet: to get a site script from a list</H5>
{% highlight powershell %}
Get-SPOSiteScriptFromList -ListUrl "https://TENANT-admin.sharepoint.com"
{% endhighlight %}

<hr>

<H2>Solution:</H2>
<p>Use the `createSiteColumnXml` verb to enable you to add in the following properties:</p>

`ShowInDisplayForm="TRUE"`

`ShowInEditForm="TRUE"`

`ShowInNewForm="TRUE"`

<p>These three properties will hide the column from the respective forms in the same way that setting a column to 'hidden' will. Below is a full JSON site script that will create a hidden column.</p>

<H5>JSON Snippet:</H5>
{% highlight json %}
{
    "$schema": "https://developer.microsoft.com/json-schemas/sp/site-design-script-actions.schema.json",
    "actions": [
      {
        "verb": "createSiteColumnXml",
        "schemaXml": "<Field Type=\"Text\" Name=\"siteColumnHiddenText\" DisplayName=\"Hidden Text\" ID=\"{162cfd59-21f1-4154-81ef-04b4d554a326}\" Required=\"FALSE\" StaticName=\"siteColumnHiddenText\" Group=\"My Custom\" EnforceUniqueValues=\"FALSE\" Customization=\"\" ShowInDisplayForm=\"FALSE\" ShowInEditForm=\"FALSE\" ShowInNewForm=\"FALSE\" />",
        "pushChanges": true
      }
    ]
  }
{% endhighlight %}

<H3>A note on the Hidden property:</H3>

`Hidden="TRUE`

<p>It may seem logical to use the 'Hidden' property instead of the three listed above as this is the same setting you would select in the GUI. However setting this tag to true will hide it from everywhere including the list settings page and the content type settings page, effectively turning it into a ghost column. This property would be useful for creating hidden calculated columns used for scripting or reporting purposes.</p>

<hr>

<H2>References</H2>

The following properties from the [Microsoft Docs SharePoint reference site][field-element] helped me solve this.

| Attribute  | Description  |
|:---|:---|
| ShowInDisplayForm  | Optional Boolean. TRUE to display the field in the form for viewing the item.  |
| ShowInEditForm  | Optional Boolean. TRUE to display the field in the form for editing the item.  |
| ShowInNewForm  | Optional Boolean. If FALSE, the field does not show up in a Fields enumeration when the display mode is set to New. Fields with this setting do not show up in the default New Item page for a given list. In particular, this is used to hide fields on the page for uploading documents to the document library.  |
| Hidden  | Optional Boolean. If TRUE, the field is completely hidden from the user interface. Setting ReadOnly to TRUE means the field is not displayed in New or Edit forms but can still be included in views.  |


[field-element]: https://docs.microsoft.com/en-gb/sharepoint/dev/schema/field-element-field