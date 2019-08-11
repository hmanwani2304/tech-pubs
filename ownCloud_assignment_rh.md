# Introduction

This guide describes the information about installing, configuring, and connecting to the ownCloud server. ownCloud is an open-source file sharing and synchronization solution, providing a secure and compliant platform to large enterprises and service providers.

# Installation and Configuration

This section describes the installation process for ownCloud with Docker. To install ownCloud manually, please refer [Manual Installation Link](https://doc.owncloud.org/server/10.2/admin_manual/installation/manual_installation.html).

## System Requirements

Before starting the installation, ensure that you have a system with the recommended environment as follows:

Platform         | Option
---------------- | ---------------------------------------
Operating System | Ubuntu 18.04 LTS
Database         | MariaDB 10+
Web Server       | Apache 2.4 with `prefork` and `mod_php`
PHP Runtime      | 7.2

For more information, please refer [System Requirements for ownCloud](https://doc.owncloud.org/server/10.2/admin_manual/installation/system_requirements.html).

## Installing ownCloud with Docker

You can install ownCloud with Docker, using the official ownCloud Docker image, which accesses the data volume in the host filesystem with separate _MariaDB_ and _Redis_ containers.

### Installing ownCloud on a Local Machine

Perform the following steps to start the installation on your local machine.

1. Create a new project directory.

  ```
  mkdir owncloud-docker-server

  cd owncloud-docker-server
  ```

2. Copy `docker-compose.yml` from the GitHub repository.

  ```
  wget https://raw.githubusercontent.com/owncloud/docs/master/modules/admin_manual/examples/installation/docker/docker-compose.yml
  ```

3. Create the environment configuration file.

  ```
  cat << EOF > .env
  OWNCLOUD_VERSION=10.0
  OWNCLOUD_DOMAIN=localhost
  ADMIN_USERNAME=admin
  ADMIN_PASSWORD=admin
  HTTP_PORT=8080
  EOF
  ```

  Also, you can enable the users to connect to the ownCloud server using its IP address with port 8080\. For example, if ownCloud server's IP address is 192.168.1.50, then add `192.168.1.50` as `OWNCLOUD_DOMAIN` value.

  ```
  cat << EOF > .env
  OWNCLOUD_VERSION=10.0
  OWNCLOUD_DOMAIN=192.168.1.50
  ADMIN_USERNAME=admin
  ADMIN_PASSWORD=admin
  HTTP_PORT=8080
  EOF
  ```

4. Build and start the container.

  ```
  docker-compose up -d
  ```

5. Verify that the containers are running successfully. To verify run `docker-compose ps`.

  **NOTE:** When all containers are running successfully, the ownCloud takes some time to be completely functional. If you run `docker-compose logs --follow owncloud` and see a significant amount of information logging to the console, then wait until it slows down to attempting the access of web UI.

## Logging In to the Web UI

To log in to the ownCloud UI:

1. Go to <http://localhost:8080>.

  **NOTE:** In case the server's IP address is added as the value of `OWNCLOUD_DOMAIN` in the `.env` file, then go to <http://IP_Address:8080>. For example, <http://192.168.1.50:8080>.

  The standard ownCloud login screen appears.

  ![ownCloud UI](https://doc.owncloud.com/server/10.2/admin_manual/_images/docker/owncloud-ui-login.png)

2. Enter the admin username and password, which you stored in `.env` file, and press Enter.

# User Management

This section describes the primary user management tasks in ownCloud, including creating a new user, resetting the password and deleting user. For more information, please refer to [User Management](https://doc.owncloud.org/server/10.2/admin_manual/configuration/user/user_configuration.html).

## Creating a New User

To create a new user:

1. Enter the new user's **Login Name** and related **E-Mail**.
2. Assign **Groups** memberships. This step is optional.
3. Click the **Create** button.

  ![Adding new user](/images/2019/08/users-page-new-user.png)

**TIP:** The Login names may contain letters (a-z, A-Z), numbers (0-9), and special characters.

## Resetting the Password

You can set a new user password in ownCloud, however, you cannot recover one. To set a new user password:

1. Hover the cursor over the user's **Password** field.

2. Select the **[Pencil]** icon.

3. Enter the user's new password in the **Password** field.

  **NOTE:** Ensure that you provide the password to the user.

![Password Reset](/images/2019/08/users-page-new-password.png)

## Deleting a User

To delete a user:

1. Open the **Users** page.

2. Hover your cursor over a user's name.

3. Click the **[trashcan]** icon.

  A confirmation dialog appears. Select **Yes** to delete the user and related files permanently, otherwise select **No**.

![Delete User](/images/2019/08/delete-user-confirmation.png)

# ownCloud Desktop and Mobile Client

ownCloud provides a secure and compliant file sharing and synchronization solution on the servers that users can control. Users can share more than one file and folders on their computer, and synchronize the files with their ownCloud server using the ownCloud Desktop and Mobile client.

## ownCloud Desktop Client

A user can download ownCloud Desktop Client from the [ownCloud Download Page](https://owncloud.com/download/#desktop-clients). The Desktop client is available for Windows, macOS, and Linux.

### Installing the ownCloud Desktop Client

This section describes the installation for **Windows**, which is same as for any other software application. However, **Linux** users must follow the instructions provided on the [ownCloud Download Page](https://owncloud.com/download/#desktop-clients), which helps in adding an appropriate repository for their Linux distribution, installing the signing key and accessing the package managers to install the desktop client.

#### System Requirements

- Windows 7+
- Mac OS X 10.7+ (64-bit only)
- CentOS 6 & 7 (64-bit only)
- Debian 8.0 & 9.0
- Fedora 25 & 26 & 27
- Ubuntu 16.04 & 17.04 & 17.10
- openSUSE Leap 42.2 & 42.3

#### Installation

After meeting the above system requirements, perform the following steps to install the desktop client:

1. Run following command to install the client.

  ```
  msiexec /passive /i ownCloud-x.y.z.msi ADDDEFAULT=Client
  ```

2. Pass the LAUNCH property to launch the client automatically after installation.

  ```
  msiexec /i ownCloud-x.y.z.msi LAUNCH="1"
  ```

  **NOTE:** This option does not have any effect without GUI.

##### Installation Wizard

The installation wizard enables the user to perform the configuration and account setup process. When the installation wizard launches:

1. Enter the URL of ownCloud server, click **Next**.

  ![client-install1](/images/2019/08/client-1.png)

2. Enter the ownCloud credentials, click **Next**.

  ![client Installation](/images/2019/08/client-2.png)

3. Sync all the files on the ownCloud server or select individual folders.

  **TIP:** The user can also change the local folder, which displays ownCloud as a default value.

  ![Client Installation Sync](/images/2019/08/client-3.png)

4. Click **Connect**.

  The client connects to the ownCloud server and displays two buttons to:

  - Connect to the ownCloud web GUI.
  - Open the local folder.

  Also, it starts synchronizing the files.

For more information about Desktop Client, please refer [Desktop Client Manual](https://doc.owncloud.com/desktop/2.5/).

## ownCloud Mobile Client

The ownCloud mobile client provides a simplified interface to the users to synchronize and share the files with other ownCloud users and groups.

### Installing Android App

The user can install the ownCloud Android app by logging to the ownCloud server from an Android device using a Web Browser such as Chrome or Firefox.

A screen with the download link appears, which redirects to the Google Play Store. The download link is also available on user's page in the ownCloud Web interface.

![ownCloud Mobile Client](/images/2019/08/android-1.png)

For more information and source code, please refer the [ownCloud download page](https://owncloud.org/download/#mobile).

To connect to the ownCloud server:

1. Enter the server URL.

2. Enter the ownCloud credentials.

3. Select **Connect**.

![ownCloud Server connect Mobile App](/images/2019/08/android-2.png)

### Installing iOS App

Similar to the Android apps, the user can find the download link for ownCloud iOS app on the user's page in ownCloud server when the user logs in using the Web browser in the iOS app.

After launching the ownCloud iOS app, enter the server URL and login credentials, it connects the user to the Files page.

For more information, refer to the [installation](https://owncloud.org/download/) page.
