# JSP-Web Development

# Section 1

### Step 1: Install and Configure Apache Tomcat

1. Download Apache Tomcat from the official website: [Apache Tomcat](https://tomcat.apache.org/).

2. Install and configure Apache Tomcat following the instructions provided on the website.

### Step 2: Install and Configure PostgreSQL

1. Download and install PostgreSQL from the official website: [PostgreSQL](https://www.postgresql.org/download/).

2. Follow the installation instructions for your operating system.

3. Create a new PostgreSQL database and table. You can use tools like pgAdmin or the command line. Here's an example:

   ```sql
   CREATE DATABASE mydatabase;
   \c mydatabase;
   CREATE TABLE users (
       id serial primary key,
       name varchar(255),
       email varchar(255)
   );
   INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
   ```

### Step 3: Create a JSP Page to Display Data

1. In your Apache Tomcat 'webapps' folder, create a new folder for your project, for example, 'myjspapp'.

2. Inside the 'myjspapp' folder, create a new file named `index.jsp` with the following content:

   ```jsp
   <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
   <%@ page import="java.sql.*" %>
   <!DOCTYPE html>
   <html>
   <head>
       <title>JSP with PostgreSQL</title>
   </head>
   <body>
       <h1>User Data from PostgreSQL</h1>
       
       <%
           String url = "jdbc:postgresql://localhost:5432/mydatabase";
           String user = "your_username";
           String password = "your_password";

           try {
               Class.forName("org.postgresql.Driver");
               Connection connection = DriverManager.getConnection(url, user, password);
               Statement statement = connection.createStatement();
               ResultSet resultSet = statement.executeQuery("SELECT * FROM users");

               while (resultSet.next()) {
                   out.println("<p>ID: " + resultSet.getInt("id") + "</p>");
                   out.println("<p>Name: " + resultSet.getString("name") + "</p>");
                   out.println("<p>Email: " + resultSet.getString("email") + "</p>");
               }

               resultSet.close();
               statement.close();
               connection.close();
           } catch (Exception e) {
               e.printStackTrace();
           }
       %>

   </body>
   </html>
   ```

   Replace "your_username" and "your_password" with your PostgreSQL username and password.

### Step 4: Configure Web Deployment Descriptor (web.xml)

Inside the 'WEB-INF' folder of your 'myjspapp' directory, create a 'web.xml' file with the following content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <display-name>MyJSPApp</display-name>
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
</web-app>
```

### Step 5: Deploy and Run

1. Start your PostgreSQL server.

2. Copy the 'myjspapp' folder into the 'webapps' directory of your Tomcat installation.

3. Start Tomcat.

4. Access your JSP page in the browser by visiting `http://localhost:8080/myjspapp/`.

You should see the user data retrieved from the PostgreSQL database displayed on the page.


# Section 2

In this section, we'll create a basic registration form and a confirmation page. Follow the steps below:

### Step 1: Set Up Your Project

1. Create a new folder for your project, for example, 'jsp-forms-navigation'.
2. Inside the project folder, create a 'WEB-INF' folder.

### Step 2: Create the Registration Form (index.jsp)

Inside your project folder, create a new file named `index.jsp` with the following content:

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <title>Registration Form</title>
</head>
<body>

    <h1>Registration Form</h1>

    <form action="confirmation.jsp" method="post">
        <label for="firstName">First Name:</label>
        <input type="text" id="firstName" name="firstName" required><br>

        <label for="lastName">Last Name:</label>
        <input type="text" id="lastName" name="lastName" required><br>

        <label for="email">Email:</label>
        <input type="email" id="email" name="email" required><br>

        <input type="submit" value="Submit">
    </form>

</body>
</html>
```

### Step 3: Create the Confirmation Page (confirmation.jsp)

Inside the 'WEB-INF' folder of your project, create a new file named `confirmation.jsp` with the following content:

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <title>Confirmation Page</title>
</head>
<body>

    <h1>Confirmation Page</h1>

    <p>Thank you for registering!</p>

    <%
        String firstName = request.getParameter("firstName");
        String lastName = request.getParameter("lastName");
        String email = request.getParameter("email");
    %>

    <p>First Name: <%= firstName %></p>
    <p>Last Name: <%= lastName %></p>
    <p>Email: <%= email %></p>

    <p><a href="index.jsp">Go back to the registration form</a></p>

</body>
</html>
```

### Step 4: Configure Web Deployment Descriptor (web.xml)

Inside the 'WEB-INF' folder, create a 'web.xml' file with the following content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <display-name>FormsNavigationApp</display-name>
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
</web-app>
```

### Step 5: Deploy and Run

1. Copy your project folder into the 'webapps' directory of your Tomcat installation.
2. Start Tomcat.
3. Access the registration form in the browser by visiting `http://localhost:8080/jsp-forms-navigation/`.

Fill out the form, submit it, and you'll be redirected to the confirmation page showing the entered information

# Section 3

Certainly! Let's extend the previous example to include a simple form that allows users to update their information in the database. We'll create an additional JSP page for the update form.

### Step 1: Create the Update Form (update.jsp)

Inside your project folder, create a new file named `update.jsp` with the following content:

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <title>Update Information</title>
</head>
<body>

    <h1>Update Information</h1>

    <form action="updateConfirmation.jsp" method="post">
        <label for="firstName">First Name:</label>
        <input type="text" id="firstName" name="firstName" value="<%= request.getAttribute("firstName") %>" required><br>

        <label for="lastName">Last Name:</label>
        <input type="text" id="lastName" name="lastName" value="<%= request.getAttribute("lastName") %>" required><br>

        <label for="email">Email:</label>
        <input type="email" id="email" name="email" value="<%= request.getAttribute("email") %>" required><br>

        <input type="submit" value="Update">
    </form>

</body>
</html>
```

### Step 2: Update the Confirmation Page (updateConfirmation.jsp)

Inside the 'WEB-INF' folder of your project, update the `confirmation.jsp` file to include a link to the update form:

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <title>Confirmation Page</title>
</head>
<body>

    <h1>Confirmation Page</h1>

    <p>Thank you for registering!</p>

    <%
        String firstName = request.getParameter("firstName");
        String lastName = request.getParameter("lastName");
        String email = request.getParameter("email");
    %>

    <p>First Name: <%= firstName %></p>
    <p>Last Name: <%= lastName %></p>
    <p>Email: <%= email %></p>

    <p><a href="index.jsp">Go back to the registration form</a></p>
    <p><a href="update.jsp?firstName=<%= firstName %>&lastName=<%= lastName %>&email=<%= email %>">Update Information</a></p>

</body>
</html>
```

### Step 3: Handle Form Submission and Database Update (updateConfirmation.jsp)

Create a new file named `updateConfirmation.jsp` inside the 'WEB-INF' folder with the following content:

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %>
<!DOCTYPE html>
<html>
<head>
    <title>Update Confirmation</title>
</head>
<body>

    <h1>Update Confirmation</h1>

    <%
        String firstName = request.getParameter("firstName");
        String lastName = request.getParameter("lastName");
        String email = request.getParameter("email");

        String newFirstName = request.getParameter("newFirstName");
        String newLastName = request.getParameter("newLastName");
        String newEmail = request.getParameter("newEmail");

        String url = "jdbc:postgresql://localhost:5432/mydatabase";
        String user = "your_username";
        String password = "your_password";

        try {
            Class.forName("org.postgresql.Driver");
            Connection connection = DriverManager.getConnection(url, user, password);
            PreparedStatement statement = connection.prepareStatement("UPDATE users SET name=?, email=? WHERE name=? AND email=?");
            statement.setString(1, newFirstName);
            statement.setString(2, newEmail);
            statement.setString(3, firstName);
            statement.setString(4, email);

            int rowsUpdated = statement.executeUpdate();

            if (rowsUpdated > 0) {
                out.println("<p>Information updated successfully!</p>");
            } else {
                out.println("<p>No rows were updated.</p>");
            }

            statement.close();
            connection.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    %>

    <p><a href="index.jsp">Go back to the registration form</a></p>

</body>
</html>
```

### Step 4: Update Navigation Links

In `confirmation.jsp`, update the link to the update form to include the email parameter:

```jsp
<p><a href="update.jsp?firstName=<%= firstName %>&lastName=<%= lastName %>&email=<%= email %>">Update Information</a></p>
```

### Step 5: Deploy and Run

1. Copy your updated project folder into the 'webapps' directory of your Tomcat installation.
2. Start Tomcat.
3. Access the registration form in the browser by visiting `http://localhost:8080/jsp-forms-navigation/`.

After submitting the registration form, you can navigate to the confirmation page and then update the information by following the "Update Information" link.
