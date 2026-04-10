# Job-Portal
Job Portal HTML
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Job Portal</title>

<style>
body {
  font-family: Arial;
  background: #0f172a;
  color: white;
  margin: 0;
}



/* NAV */
header {
  display: flex;
  justify-content: space-between;
  padding: 20px;
  background: #1e293b;
}

header h1 {
  color: #38bdf8;
}

/* HERO */
.hero {
  text-align: center;
  padding: 40px;
  background: linear-gradient(to right, #38bdf8, #6366f1);
}

/* FORM */
.form {
  padding: 20px;
}

.form input {
  margin: 5px;
  padding: 10px;
}

/* SEARCH */
#search {
  margin: 20px;
  padding: 10px;
  width: 90%;
}

/* DASHBOARD */
.dashboard {
  display: flex;
  justify-content: space-around;
  padding: 20px;
}

/* JOBS */
.jobs {
  padding: 20px;
}

.job-card {
  background: #1e293b;
  margin: 10px 0;
  padding: 15px;
  border-radius: 10px;
}

button {
  margin: 5px;
  padding: 8px;
  border: none;
  cursor: pointer;
}

.apply-btn {
  background: #22c55e;
}

.delete-btn {
  background: #ef4444;
}

.status {
  padding: 5px;
  margin-top: 5px;
  display: inline-block;
  border-radius: 5px;
}

.applied {
  background: gold;
  color: black;
}

.interview {
  background: #3b82f6;
}

.rejected {
  background: #ef4444;
}
</style>

</head>
<body>

<header>
  <h1>JobPortal</h1>
</header>

<section class="hero">
  <h2>Find Your Dream Job</h2>
</section>

<input id="search" placeholder="Search jobs...">

<!-- DASHBOARD -->
<div class="dashboard">
  <div>Total Jobs: <span id="total"></span></div>
</div>

<!-- FORM -->
<div class="form">
  <h3>Post a Job</h3>
  <form id="jobForm">
    <input id="title" placeholder="Job Title" required>
    <input id="company" placeholder="Company" required>
    <input id="location" placeholder="Location" required>
    <button type="submit">Add Job</button>
  </form>
</div>

<div class="dashboard">
  <div>Total: <span id="total"></span></div>
  <div>🟡 Applied: <span id="applied"></span></div>
  <div>🔵 Interview: <span id="interview"></span></div>
  <div>🔴 Rejected: <span id="rejected"></span></div>
</div>

<div id="loginBox">
  <h2>Login</h2>
  <input id="username" placeholder="Username">
  <input id="password" type="password" placeholder="Password">
  <button onclick="login()">Login</button>
</div>

<!-- JOB LIST -->
<div class="jobs"></div>

<script>
let jobs = JSON.parse(localStorage.getItem("jobs")) || [];

const jobContainer = document.querySelector(".jobs");
const form = document.getElementById("jobForm");
const searchInput = document.getElementById("search");
const totalEl = document.getElementById("total");



// Save
function saveJobs() {
  localStorage.setItem("jobs", JSON.stringify(jobs));
}

// Render
function renderJobs(data = jobs) {
  jobContainer.innerHTML = "";

  totalEl.textContent = jobs.length;

  if (data.length === 0) {
    jobContainer.innerHTML = "<p>No jobs found</p>";
    return;
  }

  data.forEach((job, index) => {
    const div = document.createElement("div");
    div.classList.add("job-card");

    div.innerHTML = `
      <h3>${job.title}</h3>
      <p><strong>${job.company}</strong></p>
      <p>${job.location}</p>
      <button class="apply-btn" onclick="applyJob(${index})">Apply</button>
      <button class="delete-btn" onclick="deleteJob(${index})">Delete</button>
    `;

    jobContainer.appendChild(div);
  });
}

// Add job
form.addEventListener("submit", (e) => {
  e.preventDefault();

  const newJob = {
    title: title.value,
    company: company.value,
    location: location.value,
  };

  jobs.push(newJob);
  saveJobs();
  renderJobs();
  form.reset();
});

// Delete
function deleteJob(index) {
  jobs.splice(index, 1);
  saveJobs();
  renderJobs();
}

// Apply
function applyJob(index) {
  alert(`Applied to ${jobs[index].title} at ${jobs[index].company}`);
}

// Search
searchInput.addEventListener("input", () => {
  const value = searchInput.value.toLowerCase();

  const filtered = jobs.filter(job =>
    job.title.toLowerCase().includes(value) ||
    job.company.toLowerCase().includes(value) ||
    job.location.toLowerCase().includes(value)
  );

  renderJobs(filtered);
});

// Init
renderJobs();
</script>

</body>
</html>
