## More on SDK configuration

Add the configuration necessary for the SDK to plist. Create the property list file 'AppAdForce.plist' in any arbitrary location, and input the following key-value pairs.

Right click and select "New File..."

![SDK setting 01](./img01.png)

Select "Property List".

![SDK setting 02](./img02.png)

Change the name to "AppAdForce.plist", and then click the Create button.

![SDK setting 03](./img03.png)

Select the newly created property list file, right click and select "Add Row".

![SDK setting 04](./img04.png)

Input the key-value pairs.

![SDK setting 05](./img05.png)

Input the following key-value pairs.

<table>
<tr>
  <th>Key</th>
  <th>Type</th>
  <th>Value</th>
</tr>
<tr>
  <td>APP_ID</td>
  <td>String</td>
  <td>Input the value informed to you by a FOX administrator. </td>
</tr>
<tr>
  <td>SERVER_URL</td>
  <td>String</td>
  <td>Input the value informed to you by a FOX administrator. </td>
</tr>
<tr>
  <td>APP_SALT</td>
  <td>String</td>
  <td>Input the value informed to you by a FOX administrator. </td>
</tr>
<tr>
  <td>APP_OPTIONS</td>
  <td>String</td>
  <td>Please do not type anything, keep it empty.</td>
</tr>
<tr>
  <td>CONVERSION_MODE</td>
  <td>String</td>
  <td>1</td>
</tr>
<tr>
  <td>ANALYTICS_APP_KEY</td>
  <td>String</td>
  <td>Input the value informed to you by a FOX administrator. <br />If you do not use app access tracking, this need not be set.</td>
</tr>
</table>

[AppAdForce.plist sample.](/lang/en/doc/integration/ios/config_plist/AppAdForce.plist)

---
[iOS TOP](/lang/en/doc/integration/ios/README.md)
