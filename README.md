# Smart IVR - Embed Plugin SDK 🚀

התוסף (Plugin) של Smart IVR מאפשר להטמיע את מערכת הניהול של Smart IVR בתוך פרויקטים אחרים בצורה פשוטה, מאובטחת וחלקית (OAuth-like).

ה-SDK מספק ממשק נוח לחיבור משתמשים למערכת ה-IVR שלהם ישירות מתוך הממשק שלכם, תוך שמירה על חוויית משתמש רציפה (SaaS-friendly).

---

## 📦 התקנה

כדי להתחיל להשתמש בתוסף, יש לטעון את ה-SDK ב-HTML שלכם:

```html
<!-- טעינת ה-SDK (מומלץ לפני סוף ה-body) -->
<script src="https://smart-ivr.co.il/embedding.js"></script>
```
## 🚀 שימוש בסיסי

לאחר טעינת ה-SDK, האובייקט `SmartIVREmbedding` יהיה זמין גלובלית ב-`window`.

### 1. חיבור מהיר (Recommended)

הפונקציה `connect` היא הדרך המומלצת לעבוד. היא בודקת אם המערכת כבר הוטמעה בעבר (נשמר ב-`localStorage`).
- אם המערכת קיימת -> היא מחזירה קישור התחברות ישיר.
- אם המערכת לא קיימת -> היא פותחת חלונית (popup) להטמעה ראשונית.

```javascript
const systemData = {
  username: '0770000000',      // מספר המערכת
  systemId: 'proj-12345',      // מזהה אצלכם במערכת (Unique ID)
  name: 'המערכת שלי'            // שם המערכת לתצוגה
};

SmartIVREmbedding(systemData)
  .then(result => {
    if (result) {
      if (result.connected) {
        console.log('✅ המערכת כבר מחוברת. מנתב לניהול...');
        window.open(result.connectionLink, '_blank');
      } else {
        console.log('🎉 מערכת הוטמעה בהצלחה בפעם הראשונה!');
        console.log('קישור להתחברות:', result.connectionLink);
      }
    } else {
      console.log('❌ המשתמש ביטל את ההטמעה');
    }
  })
  .catch(error => {
    console.error('שגיאה בחיבור:', error);
  });
```

---

### 🔑 התחברות עם Token (OAuth Flow)

אם כבר יש לכם טוקן התחברות מה-API של Smart IVR, תוכלו להשתמש בו ישירות ללא popup:

```javascript
SmartIVREmbedding({
  token: 'YOUR_ACCESS_TOKEN',
  redirectUrl: 'https://your-site.com/callback' // אופציונלי - לאן לחזור כשהסשן נגמר
});
```

---
## 🛠 ממשק ה-API

### `SmartIVREmbedding(systemData, options)`

הפונקציה המרכזית לחיבור/הטמעה.

**פרמטרים (`systemData`):**
- `username` (String): מספר הטלפון/מערכת.
- `systemId` (String): **חובה** - מזהה ייחודי של המערכת בבסיס הנתונים שלכם.
- `password` (String, optional): סיסמת המערכת (אם ידועה).
- `apiKey` (String, optional): מפתח API (חלופה לסיסמה).
- `name` (String, optional): שם המערכת שיוצג למשתמש.

**אפשרויות (`options`):**
- `width` (Number): רוחב החלונית (ברירת מחדל: 550).
- `height` (Number): גובה החלונית (ברירת מחדל: 700).

---


## 💾 שמירת נתונים ואבטחה

- **אחסון**: ה-SDK שומר את פרטי החיבור ב-`localStorage` תחת הדומיין שלכם עבור המערכת הספציפית (`ivr_connection_link_{systemId}`).
- **הצפנה**: הנתונים המועברים ב-URL במהלך ההטמעה מוצפנים באמצעות XOR וקידוד Base64 כדי למנוע חשיפה פשוטה של פרטים רגישים.
- **PostMessage**: התקשורת בין חלון ה-Popup לאתר שלכם מתבצעת באופן מאובטח באמצעות פרוטוקול `postMessage`.

---

## 📝 דוגמה מלאה (Vue.js / React / JS)

אם אתם עובדים ב-Single Page Application, מומלץ ליצור כפתור "ניהול שיחות":

```javascript
async function handleManageIvr() {
  try {
    const result = await SmartIVREmbedding({
      username: user.phone,
      systemId: user.id
    });
    
    if (result && result.connectionLink) {
      // פתיחת מערכת הניהול בטאב חדש
      window.open(result.connectionLink, '_blank');
    }
  } catch (err) {
    alert('שגיאה בחיבור למערכת ה-IVR');
  }
}
```

---

## 📞 תמיכה

לשאלות נוספות וסיוע טכני בהטמעה, ניתן לפנות לצוות הפיתוח של **Smart IVR**.

---
*נכתב על ידי Smart IVR Systems.*
