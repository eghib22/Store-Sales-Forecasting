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


N-BEATS: 
თავდაპირველი N-BEATS მოდელი early stopping პრობლემას აწყდებოდა 18-ე ეპოქზე, რაც იწვევდა არაოპტიმალურ შედეგებს. ეს იყო patience=10-ის გამოდა ასევე მაღალი learning_rate=0.001-ის გამო.
