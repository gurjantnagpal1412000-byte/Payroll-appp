# Payroll-appp
payroll-app/
│── server.js
│── models/
│     └── Employee.js
│── routes/
│     └── employeeRoutes.js
│── package.json
{
  "name": "payroll-app",
  "version": "1.0.0",
  "description": "Payroll Management System",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^7.0.0",
    "body-parser": "^1.20.2",
    "cors": "^2.8.5"
  }
}
const express = require("express");
const mongoose = require("mongoose");
const bodyParser = require("body-parser");
const cors = require("cors");

const app = express();

app.use(cors());
app.use(bodyParser.json());

// MongoDB connection
mongoose.connect("mongodb://127.0.0.1:27017/payrollDB")
.then(() => console.log("MongoDB Connected"))
.catch(err => console.log(err));

// Routes
const employeeRoutes = require("./routes/employeeRoutes");
app.use("/api/employees", employeeRoutes);

app.listen(5000, () => {
  console.log("Server running on port 5000");
});
const mongoose = require("mongoose");

const EmployeeSchema = new mongoose.Schema({
  name: String,
  position: String,
  basicSalary: Number,
  bonus: Number,
  deduction: Number
});

// Salary calculation
EmployeeSchema.virtual("totalSalary").get(function () {
  return this.basicSalary + this.bonus - this.deduction;
});

module.exports = mongoose.model("Employee", EmployeeSchema);



const express = require("express");
const router = express.Router();
const Employee = require("../models/Employee");

// Add employee
router.post("/", async (req, res) => {
  const emp = new Employee(req.body);
  await emp.save();
  res.json(emp);
});

// Get all employees
router.get("/", async (reres) => {
  const employees = await Employee.find();
  res.json(employees);
});

// Update employee
router.put("/:id", async (req, res) => {
  const updated = await Employee.findByIdAndUpdate(req.params.id, req.body, { new: true });
  res.json(updated);
});

// Delete employee
router.delete("/:id", async (req, res) => {
  await Employee.findByIdAndDelete(req.params.id);
  res.json({ message: "Deleted" });
});

module.exports = router;





