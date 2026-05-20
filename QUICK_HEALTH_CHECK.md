# 🚀 QUICK HEALTH CHECK REFERENCE

## ✅ The Easiest Way - Open This Page!
```
Open in browser: 
task manager frontend/test.html
```
This page runs 5 automatic tests and shows green ✅ if everything is working!

---

## 🔗 Quick URL Checks

| Check | URL | Expected Result |
|-------|-----|-----------------|
| **API Running** | http://localhost:5000/ | "AI Task Manager API Running" |
| **Health Status** | http://localhost:5000/health | JSON with ✅ status |
| **Backend Up** | http://localhost:5000/api/tasks/get | (needs token) or 401 error |

---

## 🎯 Backend Verification Checklist

```
STEP 1: Start Backend
✓ Open terminal in "task manager backend"
✓ Run: npm run dev
✓ Should see:
  - "Server running on port 5000"
  - "MongoDB Connected Successfully"

STEP 2: Quick Health Check
✓ Open browser: http://localhost:5000/health
✓ Should see JSON response with ✅ status

STEP 3: Test File Check
✓ Open: task manager frontend/test.html
✓ All tests should show ✅ (green)

STEP 4: Login Test
✓ Open: task manager frontend/index.html
✓ Try to signup and login
✓ If successful → Backend is working!
```

---

## 🔴 RED FLAGS - Backend NOT Working

| Problem | Solution |
|---------|----------|
| "Cannot connect" | MongoDB not running: `mongod` |
| Port 5000 error | Kill process: `netask -ano \| findstr :5000` |
| Connection refused | Backend not started: `npm run dev` |
| CORS errors | Check server.js has `app.use(cors())` |
| 401 Unauthorized | Missing or invalid JWT token |

---

## 🧪 Browser Console Tests (Quick)

Press `F12` → Console → Paste:

```javascript
// FASTEST CHECK - This tells you everything
fetch('http://localhost:5000/health')
  .then(r => r.json())
  .then(d => {
    if(d.status.includes('✅')) 
      console.log('✅ BACKEND IS WORKING!', d);
    else 
      console.error('❌ BACKEND ISSUE', d);
  })
  .catch(() => console.error('❌ BACKEND NOT RUNNING'));
```

---

## 📊 Signs Backend is Healthy

- ✅ Terminal shows no red errors
- ✅ Can access http://localhost:5000/
- ✅ http://localhost:5000/health returns JSON
- ✅ Can signup and login on frontend
- ✅ Tasks appear in database
- ✅ No CORS errors in browser console

---

## 🚨 Common Issues

### Issue: "Server running on port 5000" BUT API doesn't work
→ MongoDB not running! Start: `mongod`

### Issue: "Error: connect ECONNREFUSED"
→ Backend not started! Run: `npm run dev`

### Issue: CORS error in browser
→ Add this to server.js after `const app = express()`:
```javascript
app.use(cors());
```

### Issue: JWT Token errors
→ Check `.env` has: `JWT_SECRET=mysecretkey`

---

## 🎯 Real-Time Monitoring

Keep these 3 open while testing:
1. **Backend Terminal** - Watch for errors
2. **test.html Page** - See green ✅ marks
3. **Browser DevTools (F12)** - Check Network tab

---

## 📱 Test Page Features

The test.html page automatically:
- ✅ Checks health endpoint
- ✅ Verifies MongoDB connection
- ✅ Tests Auth API
- ✅ Tests Tasks API
- ✅ Validates CORS
- 📊 Shows summary (Passed/Failed)

---

## 🎓 What Each Test Checks

| Test | What it Does |
|------|-------------|
| Health Check | Verifies backend is running + uptime |
| MongoDB | Tries to create test user (DB working?) |
| Auth API | Tests if /api/auth endpoints respond |
| Tasks API | Tests if /api/tasks endpoints respond |
| CORS | Checks if cross-origin requests work |

---

**TL;DR:**
1. Run: `npm run dev` in backend folder
2. Open: `test.html` in browser
3. If all green ✅ → Backend is healthy!
4. If any red ❌ → Check terminal for errors
