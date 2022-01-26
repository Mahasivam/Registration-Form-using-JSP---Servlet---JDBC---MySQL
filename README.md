# Registration-Form-using-JSP---Servlet---JDBC---MySQL

Lets Begin

Step 1: Create database table for member
Create a new database called userdb in mysql. In the userdb database create a table called member.

CREATE TABLE `member` (
  `uname` varchar(45) NOT NULL,
  `password` varchar(45) DEFAULT NULL,
  `email` varchar(45) DEFAULT NULL,
  `phone` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`uname`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
Step 2: Create a memberRegister.jsp for the user registration
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
 pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
 <form action="Register" method="post">
 <table>
 <tr>
 <td>User Name</td>
 <td><input type="text" name="uname"></td>
 </tr>
 <tr>
 <td>Password</td>
 <td><input type="password" name="password"></td>
 </tr>
 <tr>
 <td>Email</td>
 <td><input type="text" name="email"></td>
 </tr>
 <tr>
 <td>Phone</td>
 <td><input type="text" name="phone"></td>
 </tr>
 <tr>
 <td>Submit</td>
 <td><input type="submit" value="register"></td>
 </tr>
 </table>
 </form>
</body>
</html>
Step 3: Create a dto class Member.java
This is a dto or data transfer object class that has fields according to the fields in the database table.

public class Member 
{
 private String uname,password,email,phone;
 
 public Member() {
 super();
 }
 
 public Member(String uname, String password, String email, String phone) {
 super();
 this.uname = uname;
 this.password = password;
 this.email = email;
 this.phone = phone;
 }
 
 public String getUname() {
 return uname;
 }
 
 public void setUname(String uname) {
 this.uname = uname;
 }
 
 public String getPassword() {
 return password;
 }
 
 public void setPassword(String password) {
 this.password = password;
 }
 
 public String getEmail() {
 return email;
 }
 
 public void setEmail(String email) {
 this.email = email;
 }
 
 public String getPhone() {
 return phone;
 }
 
 public void setPhone(String phone) {
 this.phone = phone;
 }
 
}
Step 4: Create a Servlet named Register.java
Control should go from the jsp to a servlet, so we will create a servlet here named Register.java. When jsp form is submitted control will go to the post method.

From the post method a dao class function is called that inputs into the database using jdbc.

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
 
/**
 * Servlet implementation class Register
 */
@WebServlet("/Register")
public class Register extends HttpServlet {
 private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public Register() {
        super();
        // TODO Auto-generated constructor stub
    }
 
 /**
 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
 */
 protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
 // TODO Auto-generated method stub
 response.getWriter().append("Served at: ").append(request.getContextPath());
 }
 
 /**
 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
 */
 protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
 String uname=request.getParameter("uname");
 String password=request.getParameter("password");
 String email=request.getParameter("email");
 String phone=request.getParameter("phone");
 Member member=new Member(uname, password, email, phone);
 RegisterDao rdao=new RegisterDao();
 String result=rdao.insert(member);
 response.getWriter().println(result);
 
 
 }
 
}
Step 5: Create a Dao class RegisterDao.java
The dao class contains the jdbc code to insert into the mysql database member table.

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
 
public class RegisterDao {
 private String dburl = "jdbc:mysql://localhost:3306/userdb";
 private String dbuname = "root";
 private String dbpassword = "mysql";
 private String dbdriver = "com.mysql.jdbc.Driver";
 
 public void loadDriver(String dbDriver)
 {
 try {
 Class.forName(dbDriver);
 } catch (ClassNotFoundException e) {
 // TODO Auto-generated catch block
 e.printStackTrace();
 }
 }
 public Connection getConnection() {
 Connection con = null;
 try {
 con = DriverManager.getConnection(dburl, dbuname, dbpassword);
 } catch (SQLException e) {
 // TODO Auto-generated catch block
 e.printStackTrace();
 }
 return con;
 }
 
 public String insert(Member member) {
 loadDriver(dbdriver);
 Connection con = getConnection();
 String sql = "insert into member values(?,?,?,?)";
 String result="Data Entered Successfully";
 try {
 PreparedStatement ps = con.prepareStatement(sql);
 ps.setString(1, member.getUname());
 ps.setString(2, member.getPassword());
 ps.setString(3, member.getEmail());
 ps.setNString(4, member.getPhone());
 ps.executeUpdate();
 } catch (SQLException e) {
 // TODO Auto-generated catch block
 result="Data Not Entered Successfully";
 e.printStackTrace();
 }
 return result;
 
 }
}
Step 6: Access the jsp page from browser
Open the url http://localhost:8080/registration/memberRegister.jsp from the browser. Fill up the required fields and hit the submit. Data will be entered in to the member table.

jsp servlet jdbc registeration
jsp servlet jdbc
This completes the the jsp servlet jdbc user registration example.
