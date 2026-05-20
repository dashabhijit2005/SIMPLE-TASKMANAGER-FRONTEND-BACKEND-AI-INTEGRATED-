# 📋 BACKEND LOGS REFERENCE GUIDE

## What You Should See in Terminal When Starting Backend

### ✅ HEALTHY STARTUP (What to expect)
```
> task-manager-backend@1.0.0 dev
> nodemon server.js

[nodemon] 3.1.10
[nodemon] to restart at any time, type `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,json
Server running on port 5000
MongoDB Connected Successfully
```

**Interpretation:**
- ✅ Server started on port 5000
- ✅ MongoDB connection successful
- ✅ Ready for API requests
- ✅ Listening for file changes

---

## What Each Log Message Means

### Server Running Message
```
Server running on port 5000
```
✅ Backend is listening on http://localhost:5000
- If you see this → Backend can accept requests

### MongoDB Connected Successfully
```
MongoDB Connected Successfully
```
✅ Database connection established
- If you see this → All database operations will work

---

## 🔴 ERROR MESSAGES & FIXES

### ❌ Error: listen EADDRINUSE: address already in use :::5000
```
Error: listen EADDRINUSE: address already in use :::5000
```
**Problem:** Another app is using port 5000

**Solution (Windows CMD):**
```bash
netstat -ano | findstr :5000
# Note the PID number shown
taskkill /PID <PID_NUMBER> /F
```

**Then restart:**
```bash
npm run dev
```

---

### ❌ Error: Database Error: connect ECONNREFUSED
```
Database Error: connect ECONNREFUSED 127.0.0.1:27017
```
**Problem:** MongoDB is not running

**Solution:**
```bash
# Open NEW terminal and run:
mongod
```

Then the backend terminal should show:
```
MongoDB Connected Successfully
```

---

### ❌ Error: MONGO_URI is not defined
```
Error: Invalid URI scheme
```
**Problem:** Missing `.env` file or MONGO_URI

**Solution:**
1. Check if `.env` file exists in backend folder
2. Add this to `.env`:
```
PORT=5000
MONGO_URI=mongodb://localhost:27017/task-manager
JWT_SECRET=mysecretkey
```
3. Restart backend: `npm run dev`

---

### ❌ Error: Cannot find module 'express'
```
Error: Cannot find module 'express'
```
**Problem:** Dependencies not installed

**Solution:**
```bash
npm install
npm run dev
```

---

## 🔍 How to Read Terminal Output

### Terminal Columns Explained
```
[nodemon] 3.1.10                    ← nodemon version (auto-restart tool)
[nodemon] watching path(s): *.*    ← watching all files for changes
[nodemon] watching extensions: js  ← will restart on .js file changes
Server running on port 5000         ← YOUR SERVER STATUS
MongoDB Connected Successfully      ← YOUR DATABASE STATUS
```

---

## 📊 Terminal Output During API Requests

### When You Make an API Call

**Expected in terminal:**
```
Server running on port 5000
MongoDB Connected Successfully
(no errors = request processed successfully)
```

**With logging enabled:**
```
POST /api/auth/signup 201 15ms
GET /api/tasks/get 200 8ms
```

---

## 🆘 Debugging Checklist

If backend isn't working:

```
□ Check terminal shows "Server running on port 5000"
  └─ If not: Backend didn't start
  └─ Run: npm run dev

□ Check terminal shows "MongoDB Connected Successfully"
  └─ If not: MongoDB not running
  └─ Run: mongod (in new terminal)

□ Check .env file exists in backend folder
  └─ If not: Create it with PORT, MONGO_URI, JWT_SECRET

□ Check no red error messages in terminal
  └─ If any: Screenshot and investigate

□ Check browser can access http://localhost:5000/
  └─ If error: Backend not responding
  └─ Check firewall settings
```

---

## 📝 Adding More Detailed Logging (Advanced)

To see every API request, add to `server.js`:

```javascript
// Add after app.use(express.json())
app.use((req, res, next) => {
  console.log(`${req.method} ${req.path}`);
  next();
});
```

Then restart: `npm run dev`

Now you'll see every request:
```
POST /api/auth/signup
GET /api/tasks/get
DELETE /api/tasks/123abc
```

---

## 🎯 Terminal Behavior Guide

| Behavior | Meaning | Action |
|----------|---------|--------|
| Server starts then stops | Error on startup | Check `npm run dev` output |
| Stays running, no messages | Normal - waiting for requests | Good! Test API calls |
| Restarts after file change | nodemon working | Expected behavior |
| Hangs at startup | Waiting for connection | Check MongoDB |
| Shows red errors | Problems detected | Fix the error shown |

---

## 💡 Pro Tips

### 1. Keep Terminal Visible
Always keep backend terminal visible while testing. Most errors show here first.

### 2. Use Restart Command
If backend acts weird, type `rs` in terminal and press Enter to restart.

### 3. Check One Thing at a Time
- First: "Server running on port 5000" ✓
- Second: "MongoDB Connected Successfully" ✓
- Third: Try API calls ✓

### 4. Look at Full Error Message
Don't just see "Error" - read the full message for clues.

### 5. Terminal Word Search
Looking for specific errors? Use `Ctrl+F` in terminal to search output history.

---

## 🎓 Terminal Screenshots Guide

### ✅ Good Startup
```
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,json
Server running on port 5000          ← GOOD
MongoDB Connected Successfully       ← GOOD
(waiting for requests...)
```

### ❌ Missing MongoDB
```
Server running on port 5000          ← Server OK
Database Error: connect ECONNREFUSED ← PROBLEM: MongoDB not running
```
**Fix:** Start MongoDB in new terminal

### ❌ Port Already Used
```
Error: listen EADDRINUSE: address already in use :::5000
```
**Fix:** Kill the process using port 5000

### ❌ Missing Dependencies
```
Error: Cannot find module 'express'
```
**Fix:** Run `npm install`

---

## 🔧 Terminal Commands Quick Reference

```bash
# Start backend
npm run dev

# Start MongoDB
mongod

# Check port usage (Windows)
netstat -ano | findstr :5000

# Kill process (Windows)
taskkill /PID <NUMBER> /F

# Restart backend (while running)
rs (then Enter)

# Stop backend
Ctrl + C

# Check if port is free
netstat -ano | findstr :5000
# If empty = port is free
```

---

## 🎯 Summary: How to Know Backend is Healthy

Terminal should show:
1. ✅ "Server running on port 5000"
2. ✅ "MongoDB Connected Successfully"
3. ✅ No red error messages
4. ✅ Ready to accept requests

Browser should show:
1. ✅ http://localhost:5000/ → "AI Task Manager API Running"
2. ✅ http://localhost:5000/health → JSON response
3. ✅ Can signup and login
4. ✅ Can create and view tasks

If all above → **Backend is healthy!** 🎉
