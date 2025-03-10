# pa10

Index.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%
 Integer activeUsers = (Integer) application.getAttribute("activeUsers");
 if (activeUsers == null) {
 application.setAttribute("activeUsers", 0);
 }
%>
<!DOCTYPE html>
<html>
<head>
 <title>Login Page</title>
</head>
<body>
 <h2>Login</h2>
 <form action="welcome.jsp" method="post">
 Username: <input type="text" name="username" required>
 <input type="submit" value="Login">
 </form>
</body>
</html>
Welcome.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%
 String user = request.getParameter("username");
 if (user == null || user.trim().isEmpty()) {
 response.sendRedirect("index.jsp");
 return;
 }
 // Get the active user count
 Integer activeUsers = (Integer) application.getAttribute("activeUsers");
 if (activeUsers == null) {
 activeUsers = 0;
 }
 // Check if user limit exceeded
 if (activeUsers >= 3) {
 response.sendRedirect("error.jsp");
 return;
 }
 // Increase active user count and store username in session
 application.setAttribute("activeUsers", activeUsers + 1);
 session.setAttribute("user", user);
%>
<!DOCTYPE html>
<html>
<head>
 <title>Welcome</title>
</head>
<body>
 <h1>Welcome, <%= user %>!</h1>
 <a href="logout.jsp">Logout</a>
</body>
</html>
Error.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
 <title>Access Denied</title>
</head>
<body>
 <h1>Maximum number of users reached. Please try again later.</h1>
 <a href="index.jsp">Go Back</a>
</body>
</html>
Logout.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%
 String user = (String) session.getAttribute("user");
 if (user != null) {
 session.invalidate(); // Invalidate session
 // Decrease active user count
 Integer activeUsers = (Integer) application.getAttribute("activeUsers");
 if (activeUsers != null && activeUsers > 0) {
 application.setAttribute("activeUsers", activeUsers - 1);
 }
 }
 response.sendRedirect("index.jsp");
%>
