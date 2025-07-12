# React + Spring Boot CORS Proxy Demo

This project demonstrates how to safely bypass browser-enforced CORS restrictions by using a Spring Boot backend to proxy requests from a React frontend.

---

## 🌍 What Is CORS?

**CORS (Cross-Origin Resource Sharing)** is a browser security feature that blocks web applications running in one origin (domain + port) from making requests to a different origin **unless explicitly allowed by the server**.

Example: A frontend on `http://localhost:3000` trying to request data from `http://localhost:8080` will be blocked **unless the backend allows it via CORS headers**.

---

## 🔐 What Happens When You Add a CORS Config in Spring Boot?

In Spring Boot, adding a configuration class like the one below allows your backend to handle and respond to CORS requests properly:

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("http://localhost:3000")
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*");
    }
}
```

### 🔍 Here's What Happens Internally:

1. **Browser makes a preflight OPTIONS request** when it sees a non-simple cross-origin request.
    - E.g., method is `POST`, or header is `Content-Type: application/json`

2. **Spring Boot intercepts this request** because of the `addCorsMappings` rule.

3. **Spring Boot responds with CORS headers like:**
   ```http
   Access-Control-Allow-Origin: http://localhost:3000
   Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
   Access-Control-Allow-Headers: *
   ```

4. Browser sees this and says:
   > "Okay! This server allows me to send the actual request."

5. **Browser then proceeds with the real GET/POST call**.

Without this config, you'd get errors like:
```
Access to fetch at 'http://localhost:8080/...' from origin 'http://localhost:3000' has been blocked by CORS policy.
```

---

## 📁 Project Structure

```
cors-demo/
├── backend/       # Spring Boot app (port 8080)
│   └── src/
│   └── pom.xml
│   └── CorsConfig.java
├── frontend/      # React app (port 3000)
│   └── src/App.js
│   └── package.json
├── screenshots/   # CORS error/success images
│   └── cors-error.png
├── README.md
```

---

## 🚀 How to Run the Demo

### ▶️ Backend (Spring Boot)
```bash
cd backend
mvn spring-boot:run
```

### ▶️ Frontend (React)
```bash
cd frontend
npm install
npm start
```

Then open your browser at [http://localhost:3000](http://localhost:3000) and click the button to test the proxy call.

---

## 📸 Screenshots

### ❌ CORS Error Without Config
![cors-error.png](screenshots/cors-error.png)

### ✅ Success With Config Enabled
![cors-success.png](screenshots/cors-success.png)

---

## 🧠 Key Takeaways

- CORS is a **browser-level restriction**, not a server bug
- You must configure the server to **explicitly allow the requesting origin**
- Spring Boot makes this easy using `WebMvcConfigurer`
- CORS config ensures safe & secure communication between frontend and backend

---

## 📚 Resources

- [MDN Docs on CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
- [Spring Boot CORS Handling](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-cors)
- [React Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

---

Happy coding! 👨‍💻🚀
