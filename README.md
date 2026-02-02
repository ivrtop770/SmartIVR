# מדריך שימוש - SmartIVR Embedding

## טעינת ה-SDK

הוסף את ה-script tag ל-HTML שלך:

```html
<script src="https://smart-ivr.co.il/embedding.js"></script>
```


## שימוש בסיסי

### 1. חיבור למערכת (מומלץ) - בודק אם המערכת קיימת

```javascript
// לאחר טעינת ה-script, SmartIVREmbedding יהיה זמין ב-window

const systemData = {
  username: '0770000000',
  password: 'password123',
  apiKey: null, // או מפתח API
  systemId: '507f1f77bcf86cd799439011', // _ID מהמסד נתונים
  name: 'מערכת ייצור'
};

// חיבור למערכת - בודק אם קיימת, אם כן מחזיר קישור, אחרת פותח הטמעה
SmartIVREmbedding.connect(systemData)
  .then(result => {
    if (result) {
      if (result.connected) {
        // המערכת כבר קיימת - יש קישור התחברות
        console.log('✅ מערכת קיימת!', result.connectionLink);
        // ניתן לפתוח את הקישור
        window.open(result.connectionLink, '_blank');
      } else {
        // זו הטמעה חדשה
        console.log('הטמעה הצליחה!', result);
        console.log('מזהה מערכת:', result.systemId);
        console.log('קישור התחברות:', result.connectionLink);
      }
    } else {
      console.log('המשתמש ביטל את ההטמעה');
    }
  })
  .catch(error => {
    console.error('שגיאה:', error);
  });
```

## אפשרויות מתקדמות

### התאמת גודל החלונית

```javascript
SmartIVREmbedding.openPopup(systemData, {
  width: 600,
  height: 700
})
  .then(result => {
    // ...
  });
```

## API Reference

### `SmartIVREmbedding.connect(systemData, options)`

**מומלץ לשימוש** - בודק אם המערכת קיימת לפי systemId. אם קיימת - מחזיר קישור התחברות, אחרת פותח הטמעה.

**Parameters:**
- `systemData` (Object): פרטי המערכת
  - `username` (string): מספר מערכת
  - `password` (string, optional): סיסמה
  - `apiKey` (string, optional): מפתח API
  - `systemId` (string, **חובה**): מזהה מערכת מהפרויקט שלכם
  - `name` (string, optional): שם המערכת
- `options` (Object, optional):
  - `encryptionKey` (string): מפתח הצפנה (default: נוצר אוטומטית)
  - `width` (number): רוחב החלונית (default: 550)
  - `height` (number): גובה החלונית (default: 700)

**Returns:** Promise<Object|null>
- `success` (boolean): האם הפעולה הצליחה
- `connected` (boolean): **true** אם המערכת כבר קיימת, **false** אם זו הטמעה חדשה
- `systemIndex` (number): אינדקס המערכת
- `systemId` (string): מזהה המערכת
- `connectionLink` (string): קישור להתחברות למערכת
- `system` (Object): פרטי המערכת

**דוגמה:**
```javascript
const result = await SmartIVREmbedding.connect({
  username: '0770000000',
  password: 'password123',
  systemId: '507f1f77bcf86cd799439011'
});

if (result) {
  if (result.connected) {
    // המערכת קיימת - פתח את הקישור
    window.location.href = result.connectionLink;
  } else {
    // זו הטמעה חדשה
    console.log('הטמעה הושלמה:', result.connectionLink);
  }
}
```

