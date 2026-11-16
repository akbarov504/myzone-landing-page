# Google Sheets bilan Bog'lash Qo'llanmasi (MY ZONE Landing Page)

Saytdagi arizalar avtomatik ravishda sizning shaxsiy Google Sheet jadvalingizga tushishi uchun quyidagi 4 ta oddiy qadamni bajaring:

---

### 1-qadam: Yangi Google Sheet yarating
1. [Google Sheets](https://sheets.google.com) ga kiring va yangi bo'sh jadval (Spreadsheet) oching.
2. Birinchi qatorga (Header) quyidagi ustun nomlarini yozing:
   - **A1**: `Sana va Vaqt`
   - **B1**: `Ism Familiya`
   - **C1**: `Telefon Raqami`
   - **D1**: `Ta'lim Formati`
   - **E1**: `Xabar / Savol`
   - **F1**: `Manba`

---

### 2-qadam: Google Apps Script oching
1. Google Sheet menyusidan: **Extensions (Kengaytmalar)** ➡️ **Apps Script** ni bosing.
2. Ochilgan kod muharriridagi hamma narsani o'chirib, o'rniga quyidagi tayyor kodni joylang:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    
    // Ma'lumotlarni qabul qilish
    var fullName = e.parameter.fullName || '';
    var phoneNumber = e.parameter.phoneNumber || '';
    var learningType = e.parameter.learningType || '';
    var message = e.parameter.message || '';
    var timestamp = e.parameter.timestamp || new Date().toLocaleString('uz-UZ', { timeZone: 'Asia/Tashkent' });
    var source = e.parameter.source || 'MY ZONE Landing';

    // Jadval oxiriga yangi qator qilib qo'shish
    sheet.appendRow([
      timestamp,
      fullName,
      phoneNumber,
      learningType,
      message,
      source
    ]);

    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'success' }))
      .setMimeType(ContentService.MimeType.JSON);

  } catch (error) {
    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'error', 'error': error.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

3. Yuqoridagi **Save (Saqlash / Floppy disk belgisi)** tugmasini bosing.

---

### 3-qadam: Web App sifatida nashr qiling (Deploy)
1. Yuqori o'ng burchakdagi ko'k **Deploy (O'rnatish)** tugmasini bosing ➡️ **New deployment (Yangi o'rnatish)** ni tanlang.
2. Chap tomondagi tishli g'ildirak (Gear icon) dan **Web app** ni tanlang.
3. Sozlamalarni quyidagicha qiling:
   - **Description**: `MY ZONE Form Webhook`
   - **Execute as**: `Me (Mening nomimdan)`
   - **Who has access**: `Anyone (Har kim)` ⬅️ **BU JUDA MUHIM!**
4. **Deploy** tugmasini bosing.
5. Google ruxsat so'rasa, **Authorize Access** ni bosing, o'z Google hisobingizni tanlang, **Advanced (Kengaytirilgan)** ➡️ **Go to Untitled project (unsafe)** ➡️ **Allow (Ruxsat berish)** ni bosing.
6. Chiqqan **Web app URL** manzilini nusxalab (Copy) oling. (U taxminan shunday bo'ladi: `https://script.google.com/macros/s/AKfycbx.../exec`).

---

### 4-qadam: URL manzilni `index.html` ga qo'yish
`index.html` faylini ochib, JavaScript qismidagi ushbu qatorga nusxalangan URL manzilingizni qo'ying:

```javascript
// index.html ichida (taxminan 1495-qator):
const GOOGLE_SCRIPT_URL = 'SIZ_NUSXALAGAN_WEB_APP_URL';
```

Bo'ldi! Endi saytdan istalgan odam ariza yuborganda, ma'lumotlar bir zumda sizning Google Sheet jadvalingizga yoziladi!
