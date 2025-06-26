# Programming Fundamentals and UI Concepts

## I. Conditional Statements

### 1. if Branch Statements  
Handles multiple conditions using `if`, `else if`, and `else`:

	```java
	int num = 15;
	if (num < 10) {
	    System.out.println("Less than 10");
	} else if (num < 20) {
	    System.out.println("Between 10 and 20");
	} else {
	    System.out.println("20 or greater");
	}
	```

### 2. switch Multi-Branch  
Handles multiple equality cases:

	```java
	int day = 2;
	switch (day) {
	    case 1:
	        System.out.println("Monday");
	        break;
	    case 2:
	        System.out.println("Tuesday");
	        break;
	    // Other cases...
	    default:
	        System.out.println("Invalid day number");
	}
	```

### 3. Ternary Conditional Expression  
Compact conditional syntax:

	```java
	int a = 5, b = 3;
	int max = (a > b)? a : b;
	System.out.println("Maximum value: " + max);
	```

## II. Loop Statements - while

Executes code block while condition is true (1-10 summation):

	```java
	int sum = 0, i = 1;
	while (i <= 10) {
	    sum += i;
	    i++;
	}
	System.out.println("Sum 1-10: " + sum);
	```

## III. Data Structures and Rendering

### 1. Object Arrays  
Store multiple objects:

	```java
	class Student {
	    String name;
	    int age;
	}
	Student[] students = new Student[3];
	students[0] = new Student();
	students[0].name = "Zhang San";
	students[0].age = 20;
	```

### 2. forEach Rendering  
Render badges using forEach:

	```java
	class Badge {
	    String text;
	    public void render() {
	        System.out.println("Rendering badge: " + text);
	    }
	}
	Badge[] badges = {new Badge(), new Badge()};
	badges[0].text = "Badge 1";
	badges[1].text = "Badge 2";
	badges.forEach(Badge::render);
	```

## IV. Layout - Grid Layout  
CSS-like grid layout implementation:

	```css
	.container {
	    display: grid;
	    grid-template-columns: 100px 100px; /* Two 100px columns */
	    grid-template-rows: 50px 50px;     /* Two 50px rows */
	}
	.item {
	    background-color: lightblue;
	    border: 1px solid gray;
	}
	```

## V. Programming Features

### 1. Conditional Rendering  
JavaScript-like implementation:

	```html
	<div id="app">
	    <!-- Conditionally rendered content -->
	    <p v-if="showMessage">Conditionally displayed message.</p>
	</div>
	```

### 2. Dynamic Rendering  
React-like implementation:

	```jsx
	function App() {
	    const [count, setCount] = useState(0);
	    return (
	        <div>
	            <p>Current count: {count}</p>
	            <button onClick={() => setCount(count + 1)}>Increment</button>
	        </div>
	    );
	}
	```

## VI. UI Elements

### 1. Overlay Mask  
Semi-transparent overlay:

	```css
	.mask {
	    position: absolute;
	    top: 0;
	    left: 0;
	    width: 100%;
	    height: 100%;
	    background-color: rgba(0, 0, 0, 0.5); /* Semi-transparent black */
	    z-index: 999;
	}
```

### 2. Fade-in Animation  
Smooth appearance effect:

	```css
	.fade-in {
	    animation: fadeIn 2s ease-in-out;
	}
	
	@keyframes fadeIn {
	    from { opacity: 0; }
	    to   { opacity: 1; }
	}
	```

### 3. Swiper Carousel  
Content/image rotation for enhanced browsing

## VII. Styling Concepts  
- `@Extend`: Style inheritance  
- `@Styles`: Reusable style definitions  
- `@Builder`: Custom style generators  

## VIII. Containers - Scroll Containers  
Provides scrolling capability when content exceeds container size