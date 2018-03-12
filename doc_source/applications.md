# Applications<a name="applications"></a>

The **Applications** page lists your applications and allows you to view the servers that compose them\. Each table entry represents an application and displays the application ID, application name, description, number of member servers, and creation time\. You can search applications, create applications, and delete applications\. On the details page for each application, you can add and remove servers\.

On this page, you can: 

+ [View and search applications](#view_apps)

+ [Create an application](#create_app)

+ [Delete one or more applications](#delete_app)

+ [Delete one or more servers from an application](#delete_servers_from_app)<a name="view_apps"></a>

**To view and search applications**

1. In the navigation menu, choose **Applications**\.

1. In the table, for **Application ID**, select an application with a non\-zero number of member servers\. A details page with a list of all the member servers displays\.

1. On the details page of the selected application, choose the **Server ID** of a member server\. From the details page for that server, you can perform management operations for the selected server described in [Servers](discovered_servers.md), including deleting it from the application\. To delete servers in bulk from an application, see [To delete one or more servers from an application](#delete_servers_from_app) below\.

The table now displays only the entries that match your filter criterion\. You can also define multiple filters, delete filters, and bypass the filter menus by typing into the filter bar directly\.<a name="create_app"></a>

**To create an application**

1. In the navigation menu, choose **Applications**\.

1. Choose **Create new application**\.

1. In the** Create new application** window, provide values for **Application name** and \(optionally\) **Application description**\. 

1. When done, choose **Create**\. Note that the newly created application is listed in the table\.<a name="delete_app"></a>

**To delete one or more applications**

1. In the navigation menu, choose **Applications**\.

1. In the table of applications, select the check boxes of applications to delete\.

1. Choose **Delete applications**\.

1. In the **Delete application** window, choose **Delete**\. Note that entries for deleted applications have been removed from the table\.<a name="delete_servers_from_app"></a>

**To delete one or more servers from an application**

1. In the navigation menu, choose **Applications**\.

1. In the table, for **Application ID**, select an application with a non\-zero number of member servers\. A details page with a list of all the member servers displays\.

1. On the details page of the selected application, select the check boxes of servers to delete from the application\.

1. When done, choose **Remove server from application**\.

1. In the **Remove server from applications** window, when you are sure you want to proceed, choose **Remove**\. Note that entries for deleted servers have been removed from the table\.