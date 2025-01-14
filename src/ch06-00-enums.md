# מבחרים והתאמת דפוסים

בפרק זה נתמקד *במבחרים* (enumerations, enums). מבחרים מאפשרים להגדיר טיפוס ע"י ציון כל *הווריאנטים* (variants) האפשריים שלו. ראשית, נגדיר ונעשה שימוש במבחר כדי להראות כיצד מבחר מקודד משמעות לצד דאטה. לאחר מכן, נביט במבחר שימושי ספציפי בשם `Option`, שמבטא מצב בוא ערך יכול להיות או משהו או כלום. אח"כ נראה כיצד התאמת דפוס בביטוי `match` מאפשרת בקלות להריץ פיסות קוד שונות עבור ערכים שונים של מבחר. לבסוף, נסביר כיצד תצורת ה- `if let` מהווה אידיאומה נוחה ופשוטה לטיפול במבחרים בקוד שלכם.
