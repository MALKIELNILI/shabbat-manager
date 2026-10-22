# Shabbat Manager - תיקונים ונוסחאות

## תאריך: 22.10.2026

### יתרה לקבלה הנכונה: ₪24,774.54

---

## הנוסחה לחישוב יתרה לקבלה:

```javascript
net = earned + aharonJoint - received - expenses
```

כאשר:
- **earned** = סך המקדמות + תשלומים סופיים שמלכיאל קיבל בפועל
- **aharonJoint** = חלק אהרון בהוצאות משותפות
- **received** = קיבלתי בפועל מהמזמינים
- **expenses** = הוצאות לקיזוז

---

## החישוב של totalMalchielEarned():

```javascript
function totalMalchielEarned() {
  let total = 0;
  for (const s of (D.shabbatot||[])) {
    const c = calcShabbat(s);
    const ma = s.malchiel_adv!=null?s.malchiel_adv:D.settings.malchiel_adv;
    if (s.advance_paid) total += ma;
    if (s.final_paid) total += c.malchiel_bal;
  }
  return total;
}
```

### לוגיקה:
- **advance_paid = true** → מלכיאל קיבל **malchiel_adv** (₪2,500 או הערך שהוקצה)
- **final_paid = true** → מלכיאל קיבל **malchiel_bal** מ-calcShabbat

---

## חלוקת התשלום בשבת:

### advance_amount >= 5000:
- הכל לעובדים
- מלכיאל בסופי: 0

### advance_amount < 5000:
- advance_amount → לעובדים
- (5000 - advance_amount) → מחולק 50/50 בסופי
- השאר מהמחיר → מחולק 50/50 בסופי

---

## חלוקת צו כללית:

```
מחיר סופי (discounted) = בסיס + חדרים פרימיום - הנחות
יתרה (balance) = מחיר סופי - מקדמה
עובדים צריכים = 5,000 ₪
שארית = balance - עובדים

אם advance >= 5000:
  - עובדים מקבלים הכל מהמקדמה
  - שארית מחולקת 50/50
  
אם advance < 5000:
  - advance → עובדים בהצגה
  - (5000 - advance) → צריך בתשלום סופי
  - החלק הזה מחולק 50/50 בסופי
  - הכל האחרות = שארית מחולקת 50/50
```

---

## שבתות שהוסרו/תוקנו:

- ❌ **שלח (בדיקה)** - 2027-01-22 - **מחוקה**
  - advance: 4000
  - malchiel_adv: 2000
  - advance_paid: true
  - היתה שבת בדיקה בלבד

---

## סטטוס נוכחי:

✅ יתרה לקבלה: **₪24,774.54**
✅ קוד תוקן
✅ Shabbat Manager עובד כמו שצריך

---

## אם צריך לחזור:
1. פתח את `index.html` בדפדפן
2. עבור לטאב "כספים שלי"
3. צריך לראות ₪24,774.54 כיתרה
