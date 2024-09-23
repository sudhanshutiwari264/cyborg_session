### **Step 1: The Setup - Mission Control System**

Our task is to build a **Mission Control System** to manage the astronauts’ data on a space mission. Each astronaut has a name, role, and an ID. We will create, update, and delete their information using an API. We’ll use **Node.js** with **Express.js** for the backend and test it using **Postman**.

#### **Initialize the Project**
In your terminal, type the following commands:

1. **Create a new Node.js project**:
   ```bash
   npm init -y
   ```

2. **Install Express.js**:
   ```bash
   npm install express --save
   ```

3. **Optional: Install Nodemon** to auto-restart the server:
   ```bash
   npm install -g nodemon
   ```

---

### **Step 2: Creating the Server**

Create a file called `server.js`. This is where we'll write the code to handle the astronauts’ data for our space mission.

```javascript
const express = require('express');
const app = express();
const PORT = 3000;

// Middleware to parse JSON data
app.use(express.json());

// Sample data array to hold astronaut information
let astronauts = [];

// Starting the server
app.listen(PORT, () => {
  console.log(`Mission Control API running on port ${PORT}`);
});
```

**Story:**  
The astronauts are getting ready for their space mission, but Mission Control doesn’t have a system to manage their profiles yet. We’ll help them by building this API!

---

### **Step 3: Create (POST) Operation – Adding Astronauts**

The space agency needs to **add new astronauts** to the system. We will use a **POST** request for this.

```javascript
// Create (POST) - Add a new astronaut
app.post('/astronauts', (req, res) => {
  const newAstronaut = {
    id: astronauts.length + 1,
    name: req.body.name,
    role: req.body.role
  };
  astronauts.push(newAstronaut);
  res.status(201).json(newAstronaut);
});
```

**Story:**  
We’re adding new astronauts to the mission crew. Each astronaut has a unique role: commander, pilot, engineer, etc. The API assigns an `ID` automatically based on the number of astronauts in the list.

- **Test in Postman:**
  - **Method:** POST
  - **URL:** `http://localhost:3000/astronauts`
  - **Body (JSON):**
    ```json
    {
      "name": "Neil Armstrong",
      "role": "Commander"
    }
    ```

---

### **Step 4: Read (GET) Operation – Retrieving Astronauts**

Now that the astronauts are in the system, Mission Control needs to **view the crew list**.

```javascript
// Read (GET) - Get all astronauts
app.get('/astronauts', (req, res) => {
  res.status(200).json(astronauts);
});
```

**Story:**  
Mission Control wants to check the astronaut profiles. This **GET** request allows them to see all the astronauts who are currently part of the mission.

- **Test in Postman:**
  - **Method:** GET
  - **URL:** `http://localhost:3000/astronauts`

---

### **Step 5: Update (PUT) Operation – Updating Astronaut Information**

Sometimes, an astronaut’s role or mission parameters change. Mission Control needs the ability to **update astronaut details**.

```javascript
// Update (PUT) - Update an astronaut by ID
app.put('/astronauts/:id', (req, res) => {
  const astronautId = parseInt(req.params.id);
  const astronaut = astronauts.find(a => a.id === astronautId);
  
  if (!astronaut) {
    return res.status(404).send('Astronaut not found');
  }

  astronaut.name = req.body.name || astronaut.name;
  astronaut.role = req.body.role || astronaut.role;

  res.status(200).json(astronaut);
});
```

**Story:**  
During training, **Neil Armstrong** decides to switch roles from commander to **mission specialist**. This PUT request allows Mission Control to update astronaut details.

- **Test in Postman:**
  - **Method:** PUT
  - **URL:** `http://localhost:3000/astronauts/1`
  - **Body (JSON):**
    ```json
    {
      "name": "Neil Armstrong",
      "role": "Mission Specialist"
    }
    ```

---

### **Step 6: Delete (DELETE) Operation – Removing Astronauts**

In case of an astronaut being reassigned or retired from the mission, we need the ability to **remove them from the system**.

```javascript
// Delete (DELETE) - Remove an astronaut by ID
app.delete('/astronauts/:id', (req, res) => {
  const astronautId = parseInt(req.params.id);
  astronauts = astronauts.filter(a => a.id !== astronautId);

  res.status(200).send(`Astronaut with ID ${astronautId} removed.`);
});
```

**Story:**  
Sometimes, unexpected circumstances occur. **An astronaut might retire** before the mission starts, and Mission Control needs to update the system accordingly. This DELETE request removes the astronaut based on their `ID`.

- **Test in Postman:**
  - **Method:** DELETE
  - **URL:** `http://localhost:3000/astronauts/1`

---

### **Step 7: Final Code (server.js)**

Here’s the full code for the **Mission Control System** API:

```javascript
const express = require('express');
const app = express();
const PORT = 3000;

app.use(express.json());

let astronauts = [];

// Create (POST) - Add a new astronaut
app.post('/astronauts', (req, res) => {
  const newAstronaut = {
    id: astronauts.length + 1,
    name: req.body.name,
    role: req.body.role
  };
  astronauts.push(newAstronaut);
  res.status(201).json(newAstronaut);
});

// Read (GET) - Get all astronauts
app.get('/astronauts', (req, res) => {
  res.status(200).json(astronauts);
});

// Update (PUT) - Update an astronaut by ID
app.put('/astronauts/:id', (req, res) => {
  const astronautId = parseInt(req.params.id);
  const astronaut = astronauts.find(a => a.id === astronautId);
  
  if (!astronaut) {
    return res.status(404).send('Astronaut not found');
  }

  astronaut.name = req.body.name || astronaut.name;
  astronaut.role = req.body.role || astronaut.role;

  res.status(200).json(astronaut);
});

// Delete (DELETE) - Remove an astronaut by ID
app.delete('/astronauts/:id', (req, res) => {
  const astronautId = parseInt(req.params.id);
  astronauts = astronauts.filter(a => a.id !== astronautId);

  res.status(200).send(`Astronaut with ID ${astronautId} removed.`);
});

app.listen(PORT, () => {
  console.log(`Mission Control API running on port ${PORT}`);
});
```

---

### **Conclusion:**

**Mission Control** now has an efficient way to manage astronauts using this API. We built a system to **create**, **read**, **update**, and **delete** astronaut data. This system can be easily expanded to manage more complex data, such as mission status, equipment lists, and even space experiments!
