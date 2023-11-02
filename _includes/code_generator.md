<link rel="stylesheet" type="text/css" href="/assets/css/code_form.css">

<form id="mysqlUserForm" class="horizontal-form">
  <label for="username">Username:</label>
  <input type="text" id="username" name="username" value="admin"><br>

  <label for="host">Host:</label>
  <input type="text" id="host" name="host" value="localhost"><br>

  <label for="password">Password:</label>
  <input type="text" id="password" name="password"><br>

  <input type="button" value="Go" onclick="createMySQLUser()">
</form>

<div id="outputMessage"></div>

<script>
    function createMySQLUser() {
        const username = document.getElementById("username").value;
        const host = document.getElementById("host").value;
        const password = document.getElementById("password").value;

        const command = `CREATE USER '${username}'@'${host}' IDENTIFIED BY '${password}';`;

        const outputMessage = document.getElementById("outputMessage");
        outputMessage.innerHTML = "<pre>" + command + "</pre>";
    }
</script>
