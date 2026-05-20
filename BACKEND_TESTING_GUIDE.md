# Backend Health Check Guide

## 1️⃣ CHECK SERVER STATUS IN TERMINAL

When you run `npm run dev`, you should see:
```
Server running on port 5000
MongoDB Connected Successfully
```

✅ If you see both messages → Backend is running perfectly!
❌ If you don't see these → There's an issue

---

## 2️⃣ TEST ENDPOINTS WITH BROWSER

### Quick Health Check
Open in your browser:
```
http://localhost:5000/
```
You should see: `"AI Task Manager API Running"`

---

## 3️⃣ TEST API ENDPOINTS (Using Browser Console)

Open your browser → Press `F12` → Go to Console tab → Copy & paste:

### Test 1: Signup
```javascript
fetch('http://localhost:5000/api/auth/signup', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    name: 'Test User',
    email: 'test@example.com',
    password: 'password123'
  })
})
.then(r => r.json())
.then(data => console.log('Signup Response:', data))
.catch(e => console.error('Signup Error:', e));
```

### Test 2: Login
```javascript
fetch('http://localhost:5000/api/auth/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    email: 'test@example.com',
    password: 'password123'
  })
})
.then(r => r.json())
.then(data => {
  console.log('Login Response:', data);
  if(data.token) localStorage.setItem('token', data.token);
})
.catch(e => console.error('Login Error:', e));
```

### Test 3: Get Tasks (after login)
```javascript
const token = localStorage.getItem('token');
fetch('http://localhost:5000/api/tasks/get', {
  headers: { 'Authorization': `Bearer ${token}` }
})
.then(r => r.json())
.then(data => console.log('Tasks Response:', data))
.catch(e => console.error('Tasks Error:', e));
```

---

## 4️⃣ USE POSTMAN (Recommended)

Download Postman: https://www.postman.com/downloads/

### In Postman:
1. **Create New Request**
2. **Method**: POST
3. **URL**: `http://localhost:5000/api/auth/signup`
4. **Headers**: Add `Content-Type: application/json`
5. **Body** (JSON):
```json
{
  "name": "Test User",
  "email": "test@gmail.com",
  "password": "password123"
}
```
6. **Click Send** → Should get success response

---

## 5️⃣ CHECK NETWORK REQUESTS IN BROWSER

1. Open browser DevTools (F12)
2. Go to **Network** tab
3. Try to login on the website
4. Watch the requests:
   - Should see `/api/auth/login` request
   - Status should be `200` (success)
   - Response should include `token`

✅ Green status codes (200, 201) = Good
❌ Red status codes (400, 401, 500) = Error

---

## 6️⃣ COMMON ISSUES & FIXES

| Issue | Solution |
|-------|----------|
| "Cannot connect to server" | Make sure MongoDB is running: `mongod` |
| Port 5000 already in use | Kill process: `netstat -ano \| findstr :5000` |
| "MongoDB Connected Failed" | Check MongoDB connection string in `.env` |
| CORS errors | Check if `cors()` is enabled in server.js |
| Authentication fails | Check JWT_SECRET in `.env` |

---

## 7️⃣ ENABLE DEBUG MODE (Advanced)

Add to beginning of `server.js`:
```javascript
console.log('🔧 Debug Mode: ON');
```

Then check terminal output for detailed logs when making requests.

---

## 8️⃣ QUICK VERIFICATION CHECKLIST

- [ ] Terminal shows "Server running on port 5000"
- [ ] Terminal shows "MongoDB Connected Successfully"
- [ ] Browser shows "AI Task Manager API Running" at http://localhost:5000/
- [ ] Can signup successfully (check console for response)
- [ ] Can login and get JWT token (check localStorage)
- [ ] Can fetch tasks after login (check Network tab)
- [ ] No red errors in browser console
- [ ] No red errors in backend terminal

✅ All checked = Backend is running smoothly!

---

## 9️⃣ REAL-TIME MONITORING

Keep these 3 things open while testing:
1. **Backend Terminal** - Watch for any errors
2. **Browser DevTools** - Check Network & Console tabs
3. **Browser Application Tab** - Check localStorage for token

---

## 🔟 STEP-BY-STEP TEST FLOW

1. **Start Backend**
   ```bash
   npm run dev
   ```
   Expected: ✅ "Server running on port 5000"

2. **Start MongoDB**
   ```bash
   mongod
   ```
   Expected: ✅ No connection errors in backend terminal

3. **Test Health Check**
   ```
   Browser: http://localhost:5000/
   ```
   Expected: ✅ "AI Task Manager API Running"

4. **Test Signup**
   ```
   Use Postman or browser console test
   ```
   Expected: ✅ User created response

5. **Test Login**
   ```
   Use Postman or browser console test
   ```
   Expected: ✅ JWT token received

6. **Test Tasks API**
   ```
   Use browser console with token
   ```
   Expected: ✅ Empty array or task list

7. **Open Frontend**
   ```
   Browser: Open index.html
   ```
   Expected: ✅ Can signup, login, add tasks

---

## 🎯 EXPECTED RESPONSES

### Successful Signup
```json
{
  "message": "Signup Successful",
  "user": {
    "_id": "...",
    "name": "Test User",
    "email": "test@example.com"
  }
}
```

### Successful Login
```json
{
  "message": "Login Successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Successful Get Tasks
```json
[
  {
    "_id": "...",
    "title": "Study for 2 hours",
    "category": "Education",
    "priority": "High",
    "completed": false
  }
]
```

---

## 📞 TROUBLESHOOTING COMMANDS

```bash
# Check if port 5000 is in use (Windows)
netstat -ano | findstr :5000

# Kill process on port 5000 (Windows)
taskkill /PID <PID> /F

# Check MongoDB status (if installed)
mongod

# Check Node version
node --version

# Reinstall dependencies if issues
npm install
```

---

**Your backend is running smoothly when:**
✅ No errors in terminal
✅ Can make API calls
✅ Get expected JSON responses
✅ Frontend can signup & login
✅ Tasks appear in database

