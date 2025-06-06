# मल्टी-क्लास वर्गीकरण पर्सेप्ट्रॉन के साथ

[AI for Beginners Curriculum](https://github.com/microsoft/ai-for-beginners) से प्रयोगशाला असाइनमेंट।

## कार्य

इस पाठ में MNIST हस्तलिखित अंकों के लिए द्विआधारी वर्गीकरण के लिए विकसित किए गए कोड का उपयोग करते हुए, एक मल्टी-क्लास वर्गीकृत बनाएँ जो किसी भी अंक को पहचान सके। प्रशिक्षण और परीक्षण डेटासेट पर वर्गीकरण सटीकता की गणना करें, और भ्रम मैट्रिक्स प्रिंट करें।

## संकेत

1. प्रत्येक अंक के लिए, "यह अंक बनाम सभी अन्य अंकों" के द्विआधारी वर्गीकरण के लिए एक डेटासेट बनाएं।
2. द्विआधारी वर्गीकरण के लिए 10 विभिन्न पर्सेप्ट्रॉन का प्रशिक्षण करें (प्रत्येक अंक के लिए एक)।
3. एक फ़ंक्शन परिभाषित करें जो एक इनपुट अंक को वर्गीकृत करेगा।

> **संकेत**: यदि हम सभी 10 पर्सेप्ट्रॉन के भार को एक मैट्रिक्स में मिलाते हैं, तो हमें एक मैट्रिक्स गुणन के माध्यम से इनपुट अंकों पर सभी 10 पर्सेप्ट्रॉन को लागू करने में सक्षम होना चाहिए। सबसे संभावित अंक को तब केवल `argmax` ऑपरेशन को आउटपुट पर लागू करके पाया जा सकता है।

## नोटबुक प्रारंभ करना

[PerceptronMultiClass.ipynb](../../../../../../lessons/3-NeuralNetworks/03-Perceptron/lab/PerceptronMultiClass.ipynb) खोलकर प्रयोगशाला शुरू करें।

**अस्वीकृति**:  
यह दस्तावेज़ मशीन-आधारित एआई अनुवाद सेवाओं का उपयोग करके अनुवादित किया गया है। जबकि हम सटीकता के लिए प्रयास करते हैं, कृपया ध्यान दें कि स्वचालित अनुवादों में त्रुटियाँ या अशुद्धियाँ हो सकती हैं। मूल दस्तावेज़ को उसकी मूल भाषा में प्राधिकृत स्रोत माना जाना चाहिए। महत्वपूर्ण जानकारी के लिए, पेशेवर मानव अनुवाद की सिफारिश की जाती है। इस अनुवाद के उपयोग से उत्पन्न किसी भी गलतफहमी या गलत व्याख्या के लिए हम जिम्मेदार नहीं हैं।