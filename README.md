<!DOCTYPE html>
<html dir="rtl" lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>طلب الكيك</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f9f3f0;
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        .container {
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            padding: 30px;
            width: 90%;
            max-width: 600px;
        }
        h1 {
            color: #d86592;
            text-align: center;
            margin-bottom: 30px;
        }
        .form-group {
            margin-bottom: 25px;
        }
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: #333;
        }
        select, input[type="radio"] {
            margin-right: 10px;
        }
        .radio-group {
            display: flex;
            gap: 20px;
            margin-top: 10px;
        }
        .radio-option {
            display: flex;
            align-items: center;
        }
        button {
            background-color: #d86592;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #c25580;
        }
        .result {
            margin-top: 25px;
            padding: 15px;
            background-color: #f5f5f5;
            border-radius: 8px;
            text-align: center;
            display: none;
        }
        .price {
            font-size: 24px;
            font-weight: bold;
            color: #d86592;
        }
        .selections {
            margin-top: 10px;
            text-align: right;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>طلب الكيك</h1>
        <div class="form-group">
            <label for="size">اختر حجم الكيك:</label>
            <select id="size" class="form-control">
                <option value="small">صغير - 50 ريال</option>
                <option value="medium">متوسط - 75 ريال</option>
                <option value="large">كبير - 100 ريال</option>
                <option value="xlarge">كبير جداً - 150 ريال</option>
            </select>
        </div>
        
        <div class="form-group">
            <label>نوع الكيك:</label>
            <div class="radio-group">
                <div class="radio-option">
                    <input type="radio" id="chocolate" name="type" value="chocolate" checked>
                    <label for="chocolate">شوكولاتة (+10 ريال)</label>
                </div>
                <div class="radio-option">
                    <input type="radio" id="vanilla" name="type" value="vanilla">
                    <label for="vanilla">فانيلا (+0 ريال)</label>
                </div>
                <div class="radio-option">
                    <input type="radio" id="strawberry" name="type" value="strawberry">
                    <label for="strawberry">فراولة (+15 ريال)</label>
                </div>
            </div>
        </div>
        
        <div class="form-group">
            <label>إضافة كريمة:</label>
            <div class="radio-group">
                <div class="radio-option">
                    <input type="radio" id="cream_yes" name="cream" value="yes" checked>
                    <label for="cream_yes">نعم (+20 ريال)</label>
                </div>
                <div class="radio-option">
                    <input type="radio" id="cream_no" name="cream" value="no">
                    <label for="cream_no">لا (+0 ريال)</label>
                </div>
            </div>
        </div>
        
        <div class="form-group">
            <label>إضافات خاصة:</label>
            <div class="radio-group">
                <div class="radio-option">
                    <input type="checkbox" id="fruits" name="extras" value="fruits">
                    <label for="fruits">فواكه طازجة (+25 ريال)</label>
                </div>
                <div class="radio-option">
                    <input type="checkbox" id="nuts" name="extras" value="nuts">
                    <label for="nuts">مكسرات (+15 ريال)</label>
                </div>
                <div class="radio-option">
                    <input type="checkbox" id="caramel" name="extras" value="caramel">
                    <label for="caramel">صوص كراميل (+10 ريال)</label>
                </div>
            </div>
        </div>
        
        <button type="button" onclick="calculatePrice()">احسب السعر النهائي</button>
        
        <div class="result" id="result">
            <h3>تفاصيل الطلب</h3>
            <div class="selections" id="selections"></div>
            <p>السعر النهائي: <span class="price" id="final-price">0</span> ريال</p>
        </div>
    </div>

    <script>
        function calculatePrice() {
            // أسعار أساسية
            const sizePrices = {
                'small': 50,
                'medium': 75,
                'large': 100,
                'xlarge': 150
            };
            
            const typePrices = {
                'chocolate': 10,
                'vanilla': 0,
                'strawberry': 15
            };
            
            const creamPrice = 20;
            const extrasPrices = {
                'fruits': 25,
                'nuts': 15,
                'caramel': 10
            };
            
            // الحصول على الاختيارات
            const size = document.getElementById('size').value;
            const type = document.querySelector('input[name="type"]:checked').value;
            const cream = document.querySelector('input[name="cream"]:checked').value;
            
            // حساب السعر الأساسي
            let totalPrice = sizePrices[size] + typePrices[type];
            
            // إضافة سعر الكريمة إذا تم اختيارها
            if (cream === 'yes') {
                totalPrice += creamPrice;
            }
            
            // إضافة سعر الإضافات
            const extras = document.querySelectorAll('input[name="extras"]:checked');
            let extrasText = [];
            
            extras.forEach(extra => {
                totalPrice += extrasPrices[extra.value];
                extrasText.push(extra.nextElementSibling.textContent.split('(')[0].trim());
            });
            
            // عرض السعر النهائي
            document.getElementById('final-price').textContent = totalPrice;
            
            // عرض الاختيارات
            const sizeText = document.querySelector(`option[value="${size}"]`).textContent.split('-')[0].trim();
            const typeText = document.querySelector(`input[value="${type}"]`).nextElementSibling.textContent.split('(')[0].trim();
            const creamText = cream === 'yes' ? 'مع كريمة' : 'بدون كريمة';
            
            let selectionsHTML = `
                <p><strong>الحجم:</strong> ${sizeText}</p>
                <p><strong>النوع:</strong> ${typeText}</p>
                <p><strong>الكريمة:</strong> ${creamText}</p>
            `;
            
            if (extrasText.length > 0) {
                selectionsHTML += `<p><strong>الإضافات:</strong> ${extrasText.join('، ')}</p>`;
            }
            
            document.getElementById('selections').innerHTML = selectionsHTML;
            
            // إظهار نتيجة الحساب
            document.getElementById('result').style.display = 'block';
        }
    </script>
</body>
</html>
