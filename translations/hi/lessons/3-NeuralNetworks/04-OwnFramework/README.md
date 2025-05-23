# न्यूरल नेटवर्क का परिचय: मल्टी-लेयर्ड पर्सेप्ट्रॉन

पिछले अनुभाग में, आपने सबसे सरल न्यूरल नेटवर्क मॉडल - एक-लेयर्ड पर्सेप्ट्रॉन, एक रैखिक दो-श्रेणी वर्गीकरण मॉडल के बारे में सीखा।

इस अनुभाग में, हम इस मॉडल को एक अधिक लचीले ढांचे में विस्तारित करेंगे, जो हमें निम्नलिखित करने की अनुमति देगा:

* दो-श्रेणी के अलावा **मल्टी-क्लास वर्गीकरण** करना
* वर्गीकरण के अलावा **रेग्रेशन समस्याओं** को हल करना
* उन श्रेणियों को अलग करना जो रैखिक रूप से अलग नहीं की जा सकतीं

हम अपने स्वयं के मॉड्यूलर ढांचे को भी विकसित करेंगे जो हमें विभिन्न न्यूरल नेटवर्क आर्किटेक्चर बनाने की अनुमति देगा।

## [प्री-लेक्चर क्विज़](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/104)

## मशीन लर्निंग का औपचारिककरण

आइए मशीन लर्निंग की समस्या को औपचारिक रूप से शुरू करते हैं। मान लीजिए कि हमारे पास एक प्रशिक्षण डेटा सेट **X** है जिसमें लेबल **Y** हैं, और हमें एक मॉडल *f* बनाना है जो सबसे सटीक भविष्यवाणियाँ करेगा। भविष्यवाणियों की गुणवत्ता को **लॉस फ़ंक्शन** ℒ द्वारा मापा जाता है। निम्नलिखित लॉस फ़ंक्शन अक्सर उपयोग किए जाते हैं:

* रिग्रेशन समस्या के लिए, जब हमें एक संख्या की भविष्यवाणी करनी होती है, हम **संपूर्ण त्रुटि** ∑<sub>i</sub>|f(x<sup>(i)</sup>)-y<sup>(i)</sup>|, या **वर्ग त्रुटि** ∑<sub>i</sub>(f(x<sup>(i)</sup>)-y<sup>(i)</sup>)<sup>2</sup> का उपयोग कर सकते हैं।
* वर्गीकरण के लिए, हम **0-1 लॉस** (जो मूल रूप से मॉडल की **सटीकता** के समान है) का उपयोग करते हैं, या **लॉजिस्टिक लॉस**।

एक-लेवल पर्सेप्ट्रॉन के लिए, फ़ंक्शन *f* को एक रैखिक फ़ंक्शन *f(x)=wx+b* के रूप में परिभाषित किया गया था (यहाँ *w* वेट मैट्रिक्स है, *x* इनपुट विशेषताओं का वेक्टर है, और *b* बायस वेक्टर है)। विभिन्न न्यूरल नेटवर्क आर्किटेक्चर के लिए, यह फ़ंक्शन अधिक जटिल रूप ले सकता है।

> वर्गीकरण के मामले में, अक्सर यह वांछनीय होता है कि संबंधित श्रेणियों की संभावनाएँ नेटवर्क आउटपुट के रूप में प्राप्त हों। मनमाने संख्याओं को संभावनाओं में परिवर्तित करने के लिए (जैसे कि आउटपुट को सामान्य करने के लिए), हम अक्सर **सॉफ्टमैक्स** फ़ंक्शन σ का उपयोग करते हैं, और फ़ंक्शन *f* बन जाता है *f(x)=σ(wx+b)*

उपर्युक्त *f* की परिभाषा में, *w* और *b* को **पैरामीटर** θ=⟨*w,b*⟩ कहा जाता है। दिए गए डेटा सेट ⟨**X**,**Y**⟩, हम पूरे डेटा सेट पर कुल त्रुटि को पैरामीटर θ के एक फ़ंक्शन के रूप में गणना कर सकते हैं।

> ✅ **न्यूरल नेटवर्क प्रशिक्षण का लक्ष्य त्रुटि को कम करना है पैरामीटर θ को बदलकर**

## ग्रेडिएंट डिसेंट ऑप्टिमाइजेशन

फ़ंक्शन ऑप्टिमाइजेशन की एक प्रसिद्ध विधि **ग्रेडिएंट डिसेंट** कहलाती है। विचार यह है कि हम लॉस फ़ंक्शन का व्युत्पन्न (बहु-आयामी मामले में **ग्रेडिएंट** कहा जाता है) पैरामीटर के संदर्भ में निकाल सकते हैं, और त्रुटि को कम करने के लिए पैरामीटर को इस तरह से बदल सकते हैं। इसे निम्नलिखित के रूप में औपचारिक किया जा सकता है:

* कुछ यादृच्छिक मानों w<sup>(0)</sup>, b<sup>(0)</sup> द्वारा पैरामीटर को प्रारंभ करें
* निम्नलिखित चरण को कई बार दोहराएँ:
    - w<sup>(i+1)</sup> = w<sup>(i)</sup>-η∂ℒ/∂w
    - b<sup>(i+1)</sup> = b<sup>(i)</sup>-η∂ℒ/∂b

प्रशिक्षण के दौरान, अनुकूलन चरणों को पूरे डेटा सेट पर विचार करते हुए गणना की जानी चाहिए (याद रखें कि लॉस को सभी प्रशिक्षण नमूनों के माध्यम से एक योग के रूप में गणना की जाती है)। हालाँकि, वास्तविक जीवन में हम डेटा सेट के छोटे हिस्से को **मिनिबैचेस** कहते हैं, और डेटा के एक उपसमुच्चय के आधार पर ग्रेडिएंट की गणना करते हैं। चूंकि उपसमुच्चय हर बार यादृच्छिक रूप से लिया जाता है, इस विधि को **स्टोकास्टिक ग्रेडिएंट डिसेंट** (SGD) कहा जाता है।

## मल्टी-लेयर्ड पर्सेप्ट्रॉन और बैकप्रोपगेशन

एक-लेयर नेटवर्क, जैसा कि हमने ऊपर देखा, रैखिक रूप से अलग की जाने वाली श्रेणियों का वर्गीकरण करने में सक्षम है। एक समृद्ध मॉडल बनाने के लिए, हम नेटवर्क की कई परतों को जोड़ सकते हैं। गणितीय रूप से इसका अर्थ होगा कि फ़ंक्शन *f* अधिक जटिल रूप लेगा, और इसे कई चरणों में गणना की जाएगी:
* z<sub>1</sub>=w<sub>1</sub>x+b<sub>1</sub>
* z<sub>2</sub>=w<sub>2</sub>α(z<sub>1</sub>)+b<sub>2</sub>
* f = σ(z<sub>2</sub>)

यहाँ, α एक **गैर-रेखीय सक्रियण फ़ंक्शन** है, σ एक सॉफ्टमैक्स फ़ंक्शन है, और पैरामीटर θ=<*w<sub>1</sub>,b<sub>1</sub>,w<sub>2</sub>,b<sub>2</sub>* हैं।

ग्रेडिएंट डिसेंट एल्गोरिदम वही रहेगा, लेकिन ग्रेडिएंट की गणना करना अधिक कठिन होगा। श्रृंखला विभेदन नियम के अनुसार, हम व्युत्पन्न को इस प्रकार निकाल सकते हैं:

* ∂ℒ/∂w<sub>2</sub> = (∂ℒ/∂σ)(∂σ/∂z<sub>2</sub>)(∂z<sub>2</sub>/∂w<sub>2</sub>)
* ∂ℒ/∂w<sub>1</sub> = (∂ℒ/∂σ)(∂σ/∂z<sub>2</sub>)(∂z<sub>2</sub>/∂α)(∂α/∂z<sub>1</sub>)(∂z<sub>1</sub>/∂w<sub>1</sub>)

> ✅ श्रृंखला विभेदन नियम का उपयोग लॉस फ़ंक्शन के व्युत्पन्न को पैरामीटर के संदर्भ में निकालने के लिए किया जाता है।

ध्यान दें कि इन सभी अभिव्यक्तियों का सबसे बायाँ भाग समान है, और इस प्रकार हम प्रभावी ढंग से लॉस फ़ंक्शन से शुरू करके और "पीछे" की ओर गणना करते हुए व्युत्पन्न की गणना कर सकते हैं। इस प्रकार मल्टी-लेयर्ड पर्सेप्ट्रॉन को प्रशिक्षित करने की विधि को **बैकप्रोपगेशन**, या 'बैकप्रोप' कहा जाता है।

<img alt="compute graph" src="images/ComputeGraphGrad.png"/>

> TODO: चित्र संदर्भ

> ✅ हम अपने नोटबुक उदाहरण में बैकप्रोप के बारे में बहुत अधिक विस्तार से कवर करेंगे।

## निष्कर्ष

इस पाठ में, हमने अपनी स्वयं की न्यूरल नेटवर्क लाइब्रेरी बनाई है, और हमने इसका उपयोग एक सरल दो-आयामी वर्गीकरण कार्य के लिए किया है।

## 🚀 चुनौती

संबंधित नोटबुक में, आप मल्टी-लेयर्ड पर्सेप्ट्रॉन बनाने और प्रशिक्षित करने के लिए अपना ढांचा लागू करेंगे। आप विस्तार से देख सकेंगे कि आधुनिक न्यूरल नेटवर्क कैसे कार्य करते हैं।

[OwnFramework](../../../../../lessons/3-NeuralNetworks/04-OwnFramework/OwnFramework.ipynb) नोटबुक पर जाएँ और इसके माध्यम से काम करें।

## [पोस्ट-लेक्चर क्विज़](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/204)

## समीक्षा और आत्म-अध्ययन

बैकप्रोपगेशन एआई और एमएल में उपयोग की जाने वाली एक सामान्य एल्गोरिदम है, जिसे [अधिक विस्तार से](https://wikipedia.org/wiki/Backpropagation) अध्ययन करने के लायक है।

## [असाइनमेंट](lab/README.md)

इस प्रयोगशाला में, आपसे इस पाठ में बनाए गए ढांचे का उपयोग करके MNIST हाथ से लिखे गए अंक वर्गीकरण को हल करने के लिए कहा गया है।

* [निर्देश](lab/README.md)
* [नोटबुक](../../../../../lessons/3-NeuralNetworks/04-OwnFramework/lab/MyFW_MNIST.ipynb)

**अस्वीकृति**:  
यह दस्तावेज़ मशीन-आधारित एआई अनुवाद सेवाओं का उपयोग करके अनुवादित किया गया है। जबकि हम सटीकता के लिए प्रयास करते हैं, कृपया ध्यान दें कि स्वचालित अनुवादों में त्रुटियाँ या गलतियाँ हो सकती हैं। मूल दस्तावेज़ को उसकी मूल भाषा में प्राधिकृत स्रोत के रूप में माना जाना चाहिए। महत्वपूर्ण जानकारी के लिए, पेशेवर मानव अनुवाद की सिफारिश की जाती है। इस अनुवाद के उपयोग से उत्पन्न किसी भी गलतफहमी या गलत व्याख्या के लिए हम जिम्मेदार नहीं हैं।