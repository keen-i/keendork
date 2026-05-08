navigate at : https://keen-i.github.io/keendork/keendork.html , cheers
<div dir="rtl">

# فك الشيفرة العبقرى: الخوارزمية الثامنة – التشفير وفك التشفير  

أهلًا بيك فى قلب العظمة. لو انت طالب زينا وعايز تفهم إزاى تحول أى كلام لـ Base64 و Hex و Binary، وترجع تانى من غير ما تغمض عينك، فانت فى المكان الصح.  
الخوارزمية الثامنة فى مشروعنا دى مش زي الباقيين؛ هى **بتاع تشفير وفك تشفير مباشر**، يعنى زى ما CyberChef بيعمل بالظبط.  

---

## ١. يعنى إيه تشفير وفك تشفير؟

- **التشفير**: تحويل النص العادى لبايتات، وبعدين تمثيل البايتات بـ:  
  - **Binary**: كل بايت يبقى 8 بتات (أصفار ووحدان).  
  - **Hexadecimal**: كل بايت يتحول لعدد ست عشرى من خانتين (00 لـ FF).  
  - **Base64**: البايتات تتقسم لمجموعات 6 بت، وتتحول لـ 64 رمز (A-Z, a-z, 0-9, +, /) مع علامات = لو محتاجة.  

- **فك التشفير**: العملية العكسية؛ تاخد سلسلة Base64 أو Hex أو Binary، وترجع النص الأصلى.  
  الجميل؟ البرنامج بيكتشف الصيغة بنفسه. متحطش فى دماغك.

---

## ٢. إزاى الآلية شغالة؟

فى الكود، لما تختار الخوارزمية 8 وتضغط **COMPRESS / ENCODE**، بيتم تشغيل دالة `algo_Encoding`:

    function [compData, model, dispStr, compBits] = algo_Encoding(txt)
        bytes = uint8(txt);             % حول النص لبايتات (كل حرف رقم 0-255)
        % Binary
        binMat  = dec2bin(bytes, 8);    % كل بايت لـ 8 بت (سلسلة أفقية)
        binFlat = reshape(binMat', 1, []); % افردهم فى سلسلة واحدة
        binDisp = strjoin(cellstr(binMat), ' '); % للعرض بمسافات
        % Hexadecimal
        hexMat  = dec2hex(bytes, 2);    % كل بايت لـ 2 حرف ست عشرى
        hexFlat = reshape(hexMat', 1, []);
        hexDisp = strjoin(cellstr(hexMat), ' ');
        % Base64
        b64Str = matlab.net.base64encode(bytes); % دالة ماتلاب الجاهزة
        compData.binary = binFlat;      % خزن التلاتة فى هيكل
        compData.hex    = hexFlat;
        compData.base64 = b64Str;
        ...
    end

الجميل إن التلات صيغ بيتم تخزينهم، وده مش مهم لفك التشفير؛ فك التشفير بيشتغل مباشرة من النص المدخل.

---

## ٣. دالة فك التشفير العبقرية `universalDecode`

الدالة دى بتحاول تفهم المدخل وتفكه بذكاء، من غير ما تحددله الصيغة.  
خلينا نشوفها سطر سطر:

    function [text, format] = universalDecode(encoded)
        % المحاولة الأولى: Base64
        try
            bytes = matlab.net.base64decode(encoded); % حاول فك Base64
            text  = char(bytes);                      % حول البايتات لنص
            format = 'Base64';                        % حدد النوع
            return;
        catch
        end

- لو المدخل Base64 صحيح، `base64decode` بترجع البايتات، بنحولهم لنص، وخلاص.  
- لو المدخل مش Base64، الخطأ بيتقفلط فى `catch` ونكمل.

        % المحاولة الثانية: Hexadecimal
        if all(isstrprop(encoded, 'xdigit')) && mod(numel(encoded),2)==0
            try
                n = numel(encoded) / 2;
                raw = zeros(1, n, 'uint8');
                for k = 1:n
                    h = encoded((k-1)*2+1 : k*2);   % خد زوج من الحروف
                    raw(k) = uint8(hex2dec(h));     % حوله لرقم
                end
                text = char(raw);
                format = 'Hexadecimal';
                return;
            catch
            end
        end

- الأول نتأكد إن كل الحروف أرقام ست عشرية (0-9, A-F)، وعددهم زوجي.  
- لو لقينا حتى رقم مش ست عشرى، `all` هتدى false.  
- بعدين ناخد كل حرفين، نحولهم لرقم بـ `hex2dec`، ونحطه فى مصفوفة `uint8`.  
- نحول المصفوفة للنص.

        % المحاولة الثالثة: Binary
        if all(ismember(encoded, '01')) && mod(numel(encoded),8)==0
            try
                n = numel(encoded) / 8;
                raw = zeros(1, n, 'uint8');
                for k = 1:n
                    chunk = encoded((k-1)*8+1 : k*8); % خد 8 بتات
                    raw(k) = uint8(bin2dec(chunk));  % حولهم لرقم
                end
                text = char(raw);
                format = 'Binary';
                return;
            catch
            end
        end

- نتأكد إن السلسلة كلها 0 و1، وطولها يقبل القسمة على 8.  
- ناخد كل 8 رموز، نحولهم لرقم عشري بـ `bin2dec`، ونحطه فى المصفوفة.  
- نحول المصفوفة لنص.

        error('decode:invalid', 'اللى حطيته مش Base64 ولا Hex ولا Binary');
    end

- لو وصلنا هنا، يبقى ولا حاجة من التلات شغالة، بنطلع رسالة خطأ واضحة.

---

## ٤. اختبارات عملية – جرب بنفسك

شغل البرنامج واختار **8 - Encoding / Decoding**.

### اختبار ١: التشفير (Text → تمثيل)
- **اكتب فى الخانة**: `Hello`
- **اضغط COMPRESS / ENCODE**  
  النتيجة فى الخانة اليمين هتبقى:

  Binary : 01001000 01100101 01101100 01101100 01101111  
  Hex    : 48 65 6C 6C 6F  
  Base64 : SGVsbG8=

  بص على Base64: `SGVsbG8=` – ده نفس الكلام.

### اختبار ٢: فك تشفير Base64 مباشر  
- **امسح الخانة، واكتب**: `SGVsbG8=`  
- **اضغط DECOMPRESS / DECODE**  
  يظهرلك فى تحت: `Hello`  
  حالة البرنامج هتقولك: "Successfully decoded as Base64."

### اختبار ٣: فك تشفير Hexadecimal مباشر  
- **اكتب**: `48656C6C6F` (من غير مسافات)  
- **اضغط DECOMPRESS / DECODE**  
  يظهرلك `Hello`، والحالة تقول "Hexadecimal".

### اختبار ٤: فك تشفير Binary مباشر  
- **اكتب**: `0100100001100101011011000110110001101111` (كل 8 بت لـ H e l l o)  
- **اضغط DECOMPRESS / DECODE**  
  يطلع `Hello`، وتقوله "Binary".

### اختبار ٥: مدخل غلط  
- **اكتب**: `أنا مش كود`  
- **اضغط DECOMPRESS / DECODE**  
  هتاخد رسالة خطأ: "اللى حطيته مش Base64 ولا Hex ولا Binary". حلو؟

---

## ٥. ليه التصميم بالطريقة دى؟

- `universalDecode` بتحاول Base64 الأول لأنه الأكثر انتشاراً وسرعة.  
- Hex سهل التعرف عليه (حروف وأرقام محددة).  
- Binary أصعب شوية بس موجود. الترتيب ده بيقلل الفشل.  
- استخدام `try/catch` بدل ما نعمل تحليل يدوى معقد – بيعتمد على إن دالة فك التشفير هتغضب لو المدخل مش مظبوط.

---

## ٦. خدها قاعدة

- لو عاوز تشفر اى نص، استخدم زر COMPRESS.  
- لو عاوز تفك اى نص مشفر، استخدم زر DECOMPRESS وانت مرتاح.  
- البرنامج بيكتشف كل حاجة. اختراعات جامدة.

---

كدا انتهى الدرس. شوفلك أى Base64 من النت وجرب تفكه، هيديك الناتج فى ثانية.  
بالتوفيق يا باشمهندس.

</div>
