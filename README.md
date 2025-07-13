ამ პროექტში ვაკეთებთ Walmart მაღაზიების გაყიდვების პროგნოზირებას. 
მონაცემების გასამზადებლად შევაერთეთ მოცემული train და test დატასეტები features და stores დატასეტებთან.
დავსპლიტეთ დამერჯილი დატასეტი. ვჰენდლავთ Nan ველიუებს და კატეგორიულები ცვლადები გადაგვყავს ნუმერიქალ ცვლადებში.

XGBoost:
საუკეთესო შედეგი:
n_estimators: 2000
verbosity: 1
wmae: 2815
ოპტიმიზაციები:
დავამატე Year, Month, Week, Day, DayOfWeek, IsMonthStart, IsMonthEnd, IsWeekend, და Quarter 
დავატრენინგე მოდელი y′ = log(Weekly_Sales + 1) და პროგნოზი შევაქციე ამ ფუნქციით exp(y′) - 1, ვარიაციის სტაბილიზაციისთვის.
პარამეტრების n_estimators, max_depth, learning_rate, subsample, colsample_bytree და min_child_weight სხვადასხვა კომბინაციებიც მოსვინჯე, თუმცა wmae მივიღე 3370. შედეგად ოპტიმიზაციებამდე არსებული მოდელი უკეთეს შედეგს დებდა.

LightGBM:
n_estimators: 2000, learning_rate: 0.015, num_leaves: 70, max_depth: 14, Wmae: 2773.
n_estimators: 3000, learning_rate: 0.001, num_leaves: 80, max_depth: 14, Wmae: 5050.
n_estimators: 3000, learning_rate: 0.01, num_leaves: 80, max_depth: 14, Wmae: 2741.
n_estimators: 3500, learning_rate: 0.01, num_leaves: 80, max_depth: 14, Wmae: 2682.
ციკლური ენკოდირება სეზონური პატერნებისთვის(სეზონები ხო ციკლურია და ამიტომ სინუსის და კოსინუსის ფუნქციები გამოვიყენე ტრანსფორმაციისთვის). დავამატე ფიჩერები quarter, dayofyear, weekofyear. შევქემენი ბინარული ინდიკატორები თითოეული markdown ტიპისთვის. დავუმატე შემდეგი ფუნქციები: Total_MarkDown, რომელიც ყველა markdown მნიშვნელობის ჯამს წარმოადგენს, MarkDown_Count, რომელიც აქტიური მარკდაუნების რაოდენობას აღნიშნავს, და Has_MarkDown, რომელიც ბინარული ინდიკატორია ნებისმიერი markdown-ის არსებობისთვის. ახალი ფუნქციები: CPI_Unemployment_Ratio, რომელიც ეკონომიკური ჯანმრთელობის ინდიკატორია, CPI_Normalized, რომელიც სტანდარტიზებული CPI მნიშვნელობებია, და Unemployment_Normalized სტანდარტიზებული უმუშევრობის მაჩვენებლებისთვის. ეს იჭერს ეკონომიკურ პირობებს, რომლებიც მოქმედებს მომხმარებლის ხარჯვაზე. Lag ფუნქციები შეიცავს წინა 1, 2, 4 და 8 კვირის გაყიდვებს. ყველა lag ფუნქცია იქმნება Store და Dept მიხედვით დაჯგუფებული Weekly_Sales-ის შესაბამისი პერიოდით გადატანით. შემდეგი ფუნქციები: Sales_Mean და Sales_Std 4, 8, 12 კვირის window-ებისთვის. ეს იჭერს გაყიდვების ტენდენციებს და ცვალებადობის ნიმუშებს. ფუნქციები: Is_Christmas_Period (15-31 დეკემბრისთვის), Is_Thanksgiving_Period (20-30 ნოემბრისთვის), Is_Back_To_School (აგვისტო და სექტემბრის დასაწყისისთვის). ეს იჭერს ძირითადი შოპინგის სეზონებს განსხვავებული ნიმუშებით. ფუნქცია Store_Dept_Interaction = Store × 1000 + Dept, რომელიც იჭერს უნიკალურ მაღაზია-დეპარტამენტის კომბინაციებს. პარამეტრები შეიცავს reg_alpha=0.1 L1 რეგულარიზაციისთვის, reg_lambda=0.1 L2 რეგულარიზაციისთვის, min_split_gain=0.1 split-ების მინიმალური gain-ისთვის, min_child_weight=0.001 მინიმალური child weight-ისთვის, subsample=0.8 რიგების sampling-ისთვის, colsample_bytree=0.8 სვეტების sampling-ისთვის. ეს ამცირებს overfitting-ს და აუმჯობესებს generalization-ს. წმაე: 1877.


N-BEATS: 
თავდაპირველი N-BEATS მოდელი early stopping პრობლემას აწყდებოდა 18-ე ეპოქზე, რაც იწვევდა არაოპტიმალურ შედეგებს. ეს იყო patience=10-ის გამოდა ასევე მაღალი learning_rate=0.001-ის გამო.
