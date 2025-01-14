## תחביר מתודות

*מתודות* הן די דומות לפונקציות: מגדירים אותן עם מילת המפתח `fn` בתוספת שם, יכולים להיות להן פרמטרים וערך מוחזר, והן מכילות קוד שמבוצע כאשר המתודה נקראת ממקום אחר. בניגוד לפונקציות, מתודות מוגדרות בהקשר של מבנה (או מבחר, או תכונה, כפי שנראה [בפרק 6][enums]<!-- ignore --> וכן [בפרק 17][trait-objects]<!-- ignore -->, בהתאמה), והפרמטר הראשון שלהן הוא תמיד `self`, שמייצג מופע של המבנה עליו המתודה נקראת.

### הגדרת מתודות

הבה נשנה את הפונקציה `area` שמקבלת מופע של `Rectangle` כפרמטר, ונהפוך אותה למתודה המוגדרת על המבנה `Rectangle`, כמוצג ברשימה 5-13.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-13/src/main.rs}}
```


<span class="caption">רשימה 5-13: הגדרת המתודה `area` על המבנה `Rectangle`</span>

כדי להגדיר את הפונקציה בהקשר של `Rectangle`, נתחיל בלוק `impl` (יישום, implementation) עבור `Rectangle`. כל מה שבתוך בלוק `impl` זה יהיה מקושר עם הטיפוס `Rectangle`. אח"כ אנו מעבירים את הפונקציה `area` לאזור התחום ע"י הסוגריים המסולסלים של ה-`impl` ומשנים, בחותם המתודה, את הפרמטר הראשון (ובמקרה זה, גם היחיד) ל-`self`, ומשתמשים ב-`self` בגוף המתודה. בפונקציה `main`, היכן שקראנו לפונקציה `area` והעברנו את `rect1` כארגומנט, נוכל עתה להשתמש *בתחביר המתודות* כדי לקרוא למתודה `area` על המופע שלנו של המבנה `Rectangle`. תחביר המתודות מקשר את המופע למתודה: מוסיפים נקודה אחרי המופע ולאחריה את שם המתודה, ואז סוגריים וארגומנטים, אם יש.

בחותם של `area`, אנו משתמשים ב-`&self` במקום ב- `rectangle: &Rectangle`. ה- `&self` הוא בעצם קיצור עבור `self: &Self`. בתוך בלוק `impl`, הטיפוס `Self` הוא כינוי לטיפוס עבורו בלוק ה-`impl` מיושם. בכל המתודות חובה שיופיע פרמטר בשם `self` מטיפוס `Self` כפרמטר הראשון, וראסט מאפשרת לכם לקצר את זה פשוט ל- `self` במיקום הראשון של רשימת הפרמטרים. שימו לב שאנחנו עדיין צריכים להשתמש ב- `&` לפני ה- `self` המקוצר כדי לציין שמתודה זו שואלת את המופע של `Self`, בדיוק כפי שעשינו עם `rectangle: &Rectangle`. מתודות יכולות לקחת בעלות על `self`, לשאול את `self` בצורה מנועת-שינוי (כפי שעשינו כאן), או לשאול את `self` בצורה ברת-שינוי, בדיוק כמו עם כל פרמטר אחר.

בחרנו להשתמש ב- `&self` כאן מאותה הסיבה שהשתמשנו ב- `&Rectangle` בגרסת הפונקציה: אנחנו לא רוצים לקחת בעלות, וכל שאנו רוצים הוא לקרוא את הדאטה במבנה, לא לכתוב אליו. אם היינו רוצים, כחלק מתפקוד המתודה, לשנות את המופע עליו אנו קוראים את המתודה, אז היינו משתמשים ב- `&mut self` כפרמטר הראשון. זה די נדיר שמתודה לוקחת בעלות על המופע שלה, ז"א ש-`self` מופיע ללא הסימן `&` כפרמט הראשון; בדר"כ משתמשים בטכניקה זו כשרוצים שהמתודה תהפוך את `self` למשהו אחר ואז רוצים למנוע מהקורא למתודה מלהשתמש במופע המקורי 
לאחר הטרנספורמציה.

הסיבה העיקרית לשימוש במתודות במקום בפונקציות, מעבר ליכולת להשתמש בתחביר המתודות וזה שאין צורך לחזור על הטיפוס של `self` בחותמים של מתודות, היא סיבה ארגונית. מיקמנו בתוך בלוק `impl` את כל מה שאפשר לעשות עם מופע של טיפוס נתון, במקום להכריח משתמשים עתידיים של הקוד שלנו לחפש אחר יכולות של `Rectangle` בכל מיני מקומות בספריית הקוד שאנו מספקים.

שימו לב שאנחנו יכולים לתת למתודה את אותו השם כמו אחד מהשדות במבנה. למשל, ניתן להגדיר מתודה על `Rectangle` בשם `width`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-06-method-field-interaction/src/main.rs:here}}
```

כאן, אנחנו בוחרים שהמתודה `width` תחזיר `true` אם הערך שבשדה `width` של המופע גדול מ-`0`, ו-`false` אם הערך הוא `0`: ניתן להשתמש בשדה בתוך מתודה בעלת אותו שם לכל מטרה. בפונקציה `main`, כאשר אנחנו קוראים ל- `rect1.width` עם סוגריים, ראסט יודעת שהכוונה היא למתודה `width`. אם לא משתמשים בסוגריים, ראסט תדע שהכוונה היא לשדה `width`.

לעיתים קרובות, אבל לא תמיד, כאשר אנחנו נותנים למתודה את אותו השם כמו אחד השדות במבנה זה מכיוון שאנו רוצים שהיא תחזיר את הערך של השדה המדובר, וזהו. מתודות כאלה מכונות *getters*, ובניגוד לשפות אחרות, ראסט לא מיישמת אותן אוטומטית. מתודות `getters` כאלה הן יעילות כי הן מאפשרות לשדות להיות פרטיים בעוד המתודה היא פומבית, וכך מתאפשרת גישת קריאה-בלבד לשדה כחלק מה-API הפומבי של הטיפוס. אנחנו נדון במשמעות של פומביות ופרטיות, וכיצד להצהיר על שדות ומתודות כפומביות או פרטיות, [בפרק 7][public]<!-- ignore -->.

> ### איפה האופרטור `<-`?
> 
> ב-C וב-++C, משתמשים בשני אופרטורים שונים לקריאה למתודות: משתמשים ב-  `.` כאשר קוראים למתודה ישירות על אובייקט וב- `->` כאשר קוראים למתודה על מצביע לאובייקט, כך שצריך קודם לעשות דירף למצביע. במילים אחרות, אם  `object`  הוא מצביע, אז   `object->something()`  פועל בדומה ל- `(*object).something()`.
> 
> לראסט אין מקבילה לאופרטור  `->`; במקום זאת, לראסט יש תכונה שנקראת *הפנייה ודירף אוטומטיים*. קריאה למתודות זה אחד המקומות הבודדים בראסט בה נעשה בכך שימוש.
> 
> הינה דרך הפעולה: כאשר אתם קוראים למתודה באמצעות `object.something()`, ראסט מוסיפה אוטומטית או `&`, `&mut` או  `*`, כך ש- `object` תואם את החותם של המתודה. במילים אחרות, האפשרויות הבאות שקולות:
> 
> <!-- CAN'T EXTRACT SEE BUG https://github.com/rust-lang/mdBook/issues/1127 -->
> 
> ```rust
> # #[derive(Debug,Copy,Clone)]
> # struct Point {
> #     x: f64,
> #     y: f64,
> # }
> #
> # impl Point {
> #    fn distance(&self, other: &Point) -> f64 {
> #        let x_squared = f64::powi(other.x - self.x, 2);
> #        let y_squared = f64::powi(other.y - self.y, 2);
> #
> #        f64::sqrt(x_squared + y_squared)
> #    }
> # }
> # let p1 = Point { x: 0.0, y: 0.0 };
> # let p2 = Point { x: 5.0, y: 6.5 };
> p1.distance(&p2);
> (&p1).distance(&p2);
> ```
> 
> האופציה הראשונה יותר נקייה. יכולת זו לבצע הפנייה אוטומטית אפשרית משום שלכל מתודה יש אובייקט מקבל (receiver) ברור -- הטיפוס של  `self`. בהינתן האובייקט המקבל ושם המתודה, ראסט יכולה להבין בוודאות האם המתודה היא מתודת קריאה (`&self`), מתודה משנה (`&mut self`), או מתודה מכלה (`self`). העובדה שבראסט השאלות מבוצעות בצורה לא מפורשת זו בזמן קריאה למתודות היא גורם מכריע בקלות השימוש של בעלות מבחינה פרקטית.

### מתודות עם יותר פרמטרים

הבה נתרגל את השימוש במתודות ע"י יישום מתודה שניה על המבנה `Rectangle`. הפעם אנו רוצים שמופע של `Rectangle` ייקח מופע אחר של `Rectangle` ויחזיר `true` אם ה-`Rectangle` השני מתאים לחלוטין בתוך `self` (ה-`Rectangle` הראשון); אחרת, יש להחזיר את הערך `false`. ז"א, ברגע שנגדיר את המתודה `can_hold`, נוכל לכתוב את התכנית ברשימה 5-14.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-14/src/main.rs}}
```


<span class="caption">רשימה 5-14: שימוש במתודה (שעוד לא נכתבה) `can_hold`</span>

הפלט המצופה יראה כדלהלן, משום שמימדי `rect2` קטנים מהמימדים של `rect1`, בעוד ש- `rect3` רחב יותר מ-`rect1`:

```text
Can rect1 hold rect2? true
Can rect1 hold rect3? false
```

אנחנו יודעים שאנו רוצים להגדיר מתודה, ולכן נפנה לבלוק `impl Rectangle`. שם המתודה יהיה `can_hold`, והיא תקבל כפרמטר השאלה מנועת-שינוי למופע של `Rectangle`. אנחנו יכולים לדעת מה יהיה טיפוס הפרמטר ע"י התבוננות בקוד שקורא למתודה: `rect1.can_hold(&rect2)` מעביר את `&rect2`, שהוא הפניה מנועת שינוי ל-`rect2`, מופע של `Rectangle`. זה הגיוני מכיוון שאנחנו רק צריכים לקרוא מ- `rect2` (לו היינו צריכים לכתוב אז היה עלינו להשתמש בהפנייה ברת-שינוי), ואנחנו רוצים ש- `main` תשמור על הבעלות של `rect2` על מנת שנוכל להשתמש בו מאוחר יותר, בקריאה למתודה `can_hold`. הערך המחוזר מ- `can_hold` יהיה בוליאני, והיישום יוודא אם הרוחב והגובה של `self` גדולים מהרוחב והגובה של ה-`Rectangle` השני, בהתאמה. הבה נוסיף את המתודה החדשה `can_hold` לבלוק ה-`impl` מרשימה 5-13, כפי שמוצג ברשימה 5-15.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-15/src/main.rs:here}}
```


<span class="caption">רשימה 5-15: יישום של המתודה `can_hold` עבור `Rectangle` שמקבלת מופע שני של `Rectangle` כפרמטר</span>

כאשר נריץ קוד זה עם הפונקציה `main` מרשימה 5-15, נקבל את הפלט הרצוי. מתודות יכולות לקבל כמה פרמטרים בצורה דומה לפונקציות. פשוט מוסיפים את הפרמטרים לחותם המתודה, לאחר הפרמטר הראשון, שהוא `self`.

### פונקציות מקושרות

כל הפונקציות שמוגדרות בתוך בלוק `impl` נקראות *פונקציות מקושרות*, משום שהן מקושרות לטיפוס המצויין מייד לאחר מילת המפתח `impl`. ניתן להגדיר פונקציות מקושרות גם ללא `self` כפרמטר הראשון שלהן (ולכן אלה לא מתודות) אם הן לא צריכות מופע של הטיפוס כדי לפעול. כבר השתמשנו בפונקציה אחת כזו: הפונקציה `String::from` המוגדרת על הטיפוס `String`.

לרוב משתמשים בפונקציות מקושרות שאינן מתודות כפונקציות בונות (constructors) שמחזירות מופע חדש של המבנה. לפונקציות כאלה קוראים, בדר"כ, `new`, אבל חשוב לזכור ש-`new` אינו שם מיוחד ואינו בנוי לתוך השפה. למשל, אנחנו יכולים לבחור לספק פונקציה מקושרת בשם `square` בעלת פרמטר יחיד ולהשתמש בו עבור הרוחב והגובה, ובכל להקל אל יצירת `Rectangle` מרובע במקום להצטרך לציין את אותו ערך פעמיים:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-03-associated-functions/src/main.rs:here}}
```

מילת המפתח `Self` בערך החוזר ובגוף הפונקציה הוא כינוי עבור הטיפוס המופיע לאחר מילת המפתח `impl`, ובמקרה שלנו זה `Rectangle`.

כדי לקרוא לפונקציה מקושרת זו, משתמשים בסימן התחבירי `::` יחד עם שם המבנה; `let sq = Rectangle::square(3);` מהווה דוגמא. פונקציה זו ממוקמת לפי המבנה: בתחביר `::` משתמשים גם עבור פונקציות מקושרות וגם עבור מיקומים שנוצרים במודולים. במודולים נדון [בפרק 7][modules]<!-- ignore -->.

### ריבוי בלוקיי `impl`

לכל מבנה יכולים להיות כמה בלוקיי `impl`. למשל, הקוד ברשימה 5-15 שקול לקוד המופיע ברשימה 5-16, שבו כל מתודה נמצאת בבלוק `impl` משלה.

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-16/src/main.rs:here}}
```


<span class="caption">רשימה 5-16: שכתוב של רשימה 5-15 תוך שימוש בריבוי `impl`</span>

אין כאן סיבה אמיתית להפריד מתודות אלה לבלוקי `impl` שונים, אבל זהו תחביר תקני. בפרק 10, כאשר נדון בטיפוסים גנריים ובתכונות, נראה מצב בו ריבוי בלוקיי `impl` שימושי.

## סיכום

מבנים מאפשרים לכם ליצור טיפוסים בעלי משמעות עבור התחום שלכם. ע"י שימוש במבנים, ניתן לאגד פיסות דאטה נפרדות, אך קשורות זו לזו, לישות אחת בעלת שמות משמעותיים לדאטה כולו, ולמרכיבים השונים. בתוך בלוקיי `impl`, ניתן להגדיר פונקציות שמקושרות לטיפוס. מתודות הן סוג מסויים של פונקציות מקושרות שמאפשרות לכם לציין את ההתנהגות של מופעים של המבנה שלכם.

אבל מבנים אינם הדרך היחידה ליצור טיפוסים משלכם: הבה נפנה כעת לנושא של מבחרים בראסט בכדי להעשיר את תיבת הכלים שלכם.

[enums]: ch06-00-enums.html
[trait-objects]: ch17-02-trait-objects.md
[public]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#exposing-paths-with-the-pub-keyword
[modules]: ch07-02-defining-modules-to-control-scope-and-privacy.html
