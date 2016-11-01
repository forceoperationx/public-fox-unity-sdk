## The detail of SDK setting

Add the necessary setting for SDK's action into plist. Create the property list file, which is 「AppAdForce.plist」at arbitrary place in project and type the next key and value.

Right click at arbitrary place →Select "New File...".

![SDK setting 01](./img01.png)

Select "Property List".

![SDK setting 02](./img02.png)

Change the name to "AppAdForce.plist", click the Create button.

![SDK setting 03](./img03.png)

Choose the created property list file and open the menu by right click and select"Add Row".

![SDK setting 04](./img04.png)

Setting the each key and value.

![SDK setting 05](./img05.png)

Key and value for setting are following.

<table>
<tr>
  <th>Key</th>
  <th>Type</th>
  <th>Value</th>
</tr>
<tr>
  <td>APP_ID</td>
  <td>String</td>
  <td>There will be a contact from the administrator,so please type the value. </td>
</tr>
<tr>
  <td>SERVER_URL</td>
  <td>String</td>
  <td>There will be a contact from the administrator,so please type the value. </td>
</tr>
<tr>
  <td>APP_SALT</td>
  <td>String</td>
  <td>There will be a contact from the administrator,so please type the value. </td>
</tr>
<tr>
  <td>APP_OPTIONS</td>
  <td>String</td>
  <td>Please do not type anything, so please keep it empty.</td>
</tr>
<tr>
  <td>CONVERSION_MODE</td>
  <td>String</td>
  <td>1</td>
</tr>
<tr>
  <td>ANALYTICS_APP_KEY</td>
  <td>String</td>
  <td>There will be a contact from the administrator,so please type the value. <br />If you do not use the access analysis, does not need to be set.</td>
</tr>
</table>

[AppAdForce.plist sample.](/lang/ja/doc/integration/ios/config_plist/AppAdForce.plist)

---
[iOS TOP](/lang/ja/doc/integration/ios/README.md)
