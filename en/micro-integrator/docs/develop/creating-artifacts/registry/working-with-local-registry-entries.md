# Creating Local Registry Entries

Follow these steps to create a new local entry.

1.  If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right-click the project in the navigator and go to **New → Local Entry** to open the **New Local Entry** dialog.
4.  Select **Create a New Local Entry** and click **Next**.
5.  Type a unique name for the local entry, specify one of the following types of local entries and specify the details
    -   **In-Line Text Entry**: Type the text you want to store
    -   **In-Line XML Entry**: Type the XML code you want to store
    -   **Source URL Entry**: Type or browse to the URL you want to store
6.  Do one of the following:  
    -   To save the local entry in an existing ESB Config project in your
        workspace, click **Browse** and select that project.
    -   To save the local entry in a new ESB Config project, click
        **Create new Project** and create the new project.
7.  Click **Finish** . The local entry is created in the local-entries
    folder under the ESB config project you specified, and the local
    entry appears in the editor. Click its icon in the editor to view
    its properties.

## Using a local entry

After you create a local entry, you can reference it from a mediator in
your mediation workflow. For example, if you created a local entry with
XSLT code, you can add an XSLT mediator to the workflow and then
reference the local entry as follows:

1.  Click the XSLT mediator to view its properties, click the **XSLT
    Static Schema Key** property, and then click the browse [...]
    button on the far right of the property's value.
2.  Click the **Workspace** link, and then navigate to and select the
    local entry that contains the XSLT code.
3.  Click **OK** .

!!! Info
    If you want to add local entries before deploying the server, you can add them to the top-level bootstrap file `         synapse.xml        `, or to separate XML files in the `         local-entries        ` directory, which are located in under `MI_HOME\repository\deployment\server\synapse-configs\default        `. When the server is started, these configurations will be added to the registry.