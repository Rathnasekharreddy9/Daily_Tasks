<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Student Registration Form</title>

  <style>
    body {
      font-family: 'Poppins', sans-serif;
      background-color: #e6e6e6; /* light grey background */
      margin: 0;
      padding: 0;
    }

    .container {
      width: 55%;
      max-width: 650px;
      background-color: #fff;
      margin: 40px auto;
      padding: 45px 55px; /* reduced padding */
      border-radius: 8px;
      box-shadow: 0 0 8px rgba(0, 0, 0, 0.1);
    }

    h2 {
      text-align: center;
      color: #333;
      margin-bottom: 35px;
    }

    label {
      display: block;
      margin-bottom: 4px;
      font-weight: 600;
      color: #444;
    }

    input[type="text"],
    input[type="email"],
    input[type="tel"],
    input[type="date"],
    select,
    textarea {
      width: 100%;
      padding: 7px 9px; /* reduced padding */
      margin-bottom: 15px; /* reduced spacing between fields */
      border: 1px solid #bbb;
      border-radius: 4px;
      font-size: 14px;
      background-color: #f9f9f9; /* subtle input background color */
      transition: border-color 0.3s, background-color 0.3s;
    }

    input:focus,
    textarea:focus,
    select:focus {
      outline: none;
      border-color: #007bff;
      background-color: #eef6ff; /* light blue on focus */
    }

    textarea {
      resize: vertical;
      height: 80px;
    }

    .gender-options {
      margin-bottom: 15px;
    }

    .gender-options label {
      font-weight: normal;
      margin-right: 15px;
    }

    .btn {
      display: block;
      width: 100%;
      background-color: #0f56a3;
      color: white;
      border: none;
      padding: 10px;
      font-size: 15px;
      border-radius: 4px;
      cursor: pointer;
      transition: background-color 0.3s;
    }

    .btn:hover {
      background-color: #3e4c5c;
    }

    .footer {
      text-align: center;
      margin-top: 15px;
      color: #777;
      font-size: 13px;
    }
  </style>
</head>

<body>
  <div class="container">
    <h2>Student Registration Form</h2>

    <form action="#" method="post">
      
      <label for="name">Full Name</label>
      <input type="text" id="name" name="name" placeholder="Enter your full name" required>

      <label for="email">Email Address</label>
      <input type="email" id="email" name="email" placeholder="Enter your email" required>

      <label for="phone">Phone Number</label>
      <input type="tel" id="phone" name="phone" placeholder="Enter your phone number" required>

      <label>Gender</label>
      <div class="gender-options">
        <label><input type="radio" name="gender" value="Male" required> Male</label>
        <label><input type="radio" name="gender" value="Female"> Female</label>
        <label><input type="radio" name="gender" value="Other"> Other</label>
      </div>

      <label for="dob">Date of Birth</label>
      <input type="date" id="dob" name="dob" required>

      <label for="course">Select Course</label>
      <select id="course" name="course" required>
        <option value="">-- Choose a Course --</option>
        <option value="BSc Computer Science">BSc Computer Science</option>
        <option value="BCom">BCom</option>
        <option value="BA English">BA English</option>
        <option value="BBA">BBA</option>
        <option value="MBA">MBA</option>
      </select>

      <label for="address">Address</label>
      <textarea id="address" name="address" placeholder="Enter your address" required></textarea>

      <button type="submit" class="btn">Submit Registration</button>
    </form>

    <div class="footer">
      &copy; 2025 Student Registration Portal
    </div>
  </div>
</body>
</html>



<img width="613" height="601" alt="image" src="https://github.com/user-attachments/assets/19eb492b-b72d-4ac1-9780-13a5d65b4101" />
