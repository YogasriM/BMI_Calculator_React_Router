# Ex06 BMI Calculator
## Name: YOGASRI
## Register Number: 212224220124

## AIM
To develop a responsive and interactive Body Mass Index (BMI) Calculator using React that allows users to input their height and weight, and calculates their BMI to categorize their health status (e.g., Underweight, Normal, Overweight, Obese).

## DESIGN STEPS

### STEP 1: Initialize React Project

<li>Create a new React app using create-react-app.</li>
<li>Install React Router using:</li>
npm install react-router-dom

### STEP 2: Set Up Routing

Create routing structure with react-router-dom:

<li>Home route (/) – Intro or Navigation</li>

<li>BMI Calculator route (/bmi)</li>

<li>Result route (/result)</li>

### STEP 3: Design the BMI Form Page

<li>Create a form to accept Height (in cm or m) and Weight (in kg).</li>

<li>On form submit, navigate to the result page with entered values via URL query params or context/state.</li>

## STEP 4: Handle Input Validation

<li>Check if height and weight are valid numbers.</li>

<li>Optionally, show error messages for invalid inputs.</li>

### STEP 5: Perform BMI Calculation

<li>In the result component:

<li>Extract height and weight from the route (URL or passed state).</li>

<li>Apply the BMI formula:</li>

![image](https://github.com/user-attachments/assets/ec785506-c96b-489e-8783-fb1a5d36101a)
​
 
<li>Convert height from cm to m if needed.</li></li>

### STEP 6: Display Result

<li>Show calculated BMI.</li>

<li>Show category based on BMI range:

<li>Underweight, Normal, Overweight, Obese, etc.</li></li>

### STEP 7: Navigation Options

<li>Provide a button to go back to the BMI form to calculate again.</li>

### STEP 8: Enhancements

<li>Add styling using CSS or Tailwind.</li>

## PROGRAM

### App.jsx

```jsx
import { useMemo, useState } from "react";
import {
    Link,
    Route,
    Routes,
    useNavigate,
    useSearchParams,
} from "react-router-dom";
import "./App.css";

function Home() {
    return (
        <main className="page">
            <h1>BMI Calculator</h1>
            <p>Simple calculator for height and weight.</p>
            <Link className="button" to="/bmi">
                Open Calculator
            </Link>
        </main>
    );
}

function BmiForm() {
    const navigate = useNavigate();
    const [height, setHeight] = useState("");
    const [weight, setWeight] = useState("");
    const [unit, setUnit] = useState("cm");
    const [error, setError] = useState("");

    function handleSubmit(event) {
        event.preventDefault();
        const heightValue = Number(height);
        const weightValue = Number(weight);

        if (!(heightValue > 0) || !(weightValue > 0)) {
            setError("Enter valid height and weight values.");
            return;
        }

        navigate(
            `/result?height=${heightValue}&weight=${weightValue}&unit=${unit}`,
        );
    }

    return (
        <main className="page card">
            <h1>BMI Calculator</h1>
            <form className="form" onSubmit={handleSubmit}>
                <label>
                    Height
                    <div className="row">
                        <input
                            type="number"
                            min="0"
                            step="any"
                            value={height}
                            onChange={(event) => setHeight(event.target.value)}
                            placeholder="170"
                        />
                        <select
                            value={unit}
                            onChange={(event) => setUnit(event.target.value)}
                        >
                            <option value="cm">cm</option>
                            <option value="m">m</option>
                        </select>
                    </div>
                </label>
                <label>
                    Weight (kg)
                    <input
                        type="number"
                        min="0"
                        step="any"
                        value={weight}
                        onChange={(event) => setWeight(event.target.value)}
                        placeholder="65"
                    />
                </label>
                {error ? <p className="error">{error}</p> : null}
                <button className="button" type="submit">
                    Calculate
                </button>
            </form>
            <Link to="/">Home</Link>
        </main>
    );
}

function Result() {
    const [searchParams] = useSearchParams();
    const height = Number(searchParams.get("height"));
    const weight = Number(searchParams.get("weight"));
    const unit = searchParams.get("unit") || "cm";

    const bmi = useMemo(() => {
        if (!(height > 0) || !(weight > 0)) return null;
        const heightInM = unit === "m" ? height : height / 100;
        return weight / (heightInM * heightInM);
    }, [height, unit, weight]);

    const category =
        bmi === null
            ? ""
            : bmi < 18.5
              ? "Underweight"
              : bmi < 25
                ? "Normal"
                : bmi < 30
                  ? "Overweight"
                  : "Obese";

    return (
        <main className="page card">
            <h1>Result</h1>
            {bmi === null ? (
                <p>Invalid input. Go back and enter valid values.</p>
            ) : (
                <>
                    <p>BMI: {bmi.toFixed(2)}</p>
                    <p>Status: {category}</p>
                </>
            )}
            <Link className="button" to="/bmi">
                Back
            </Link>
        </main>
    );
}

function App() {
    return (
        <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/bmi" element={<BmiForm />} />
            <Route path="/result" element={<Result />} />
        </Routes>
    );
}

export default App;


```

### App.css

```css
.page {
    min-height: 100svh;
    display: grid;
    place-items: center;
    padding: 24px;
    gap: 16px;
}

.card {
    max-width: 420px;
    width: 100%;
    box-sizing: border-box;
}

.form {
    display: grid;
    gap: 14px;
    width: 100%;
}

label {
    display: grid;
    gap: 6px;
    text-align: left;
}

input,
select,
.button {
    font: inherit;
}

input,
select {
    padding: 10px 12px;
    border: 1px solid var(--border);
    border-radius: 8px;
    background: var(--bg);
    color: var(--text-h);
}

.row {
    display: grid;
    grid-template-columns: 1fr 90px;
    gap: 10px;
}

.button {
    display: inline-flex;
    justify-content: center;
    padding: 10px 14px;
    border: 0;
    border-radius: 8px;
    background: var(--text-h);
    color: var(--bg);
    text-decoration: none;
    cursor: pointer;
}

.error {
    color: #b91c1c;
}


```

## OUTPUT

<img width="1912" height="1190" alt="Screenshot 2026-06-01 at 1 04 38 PM" src="https://github.com/user-attachments/assets/3a5376bd-cb61-4a2b-a0b8-c6402424353c" />
<img width="1912" height="1190" alt="Screenshot 2026-06-01 at 1 04 41 PM" src="https://github.com/user-attachments/assets/16b8bb23-145c-40a5-a3d8-7475093f321c" />



## RESULT
The BMI Calculator successfully takes user input for height and weight, performs the BMI calculation in real-time using React state and event handling, and displays the BMI value along with the corresponding health category.
