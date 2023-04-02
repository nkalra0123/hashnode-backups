---
title: "Translate Hinglish to English with Google Translate API"
seoDescription: "Using google translate API to translate hindi or hinglish to English
Translate any language to any other language 
Transliteration of text"
datePublished: Sun Apr 02 2023 04:22:19 GMT+0000 (Coordinated Universal Time)
cuid: clfywc0pw000409iiehux3wpj
slug: translate-hinglish-to-english-with-google-translate-api
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/hpbiThVmsss/upload/2af7b687f69144249da575523b14110c.jpeg
tags: python, apis, google-translate, translation

---

Google Translate is a powerful tool that can help you quickly and easily translate text from one language to another. It is one of the most popular and widely used translation services globally, and it is available for free both on the web and as an API.

If you want to use this API, you'll need to know the source language and target language.

Sample code and setup instructions are available : [Quickstart: Translate text with Cloud Translation Advanced  |  Google Cloud](https://cloud.google.com/translate/docs/advanced/translate-text-advance)

```python
# Translate text from English to French
    # Detail on supported types can be found here:
    # https://cloud.google.com/translate/docs/supported-formats
    response = client.translate_text(
        request={
            "parent": parent,
            "contents": [text],
            "mime_type": "text/plain",  # mime types: text/plain, text/html
            "source_language_code": "en-US",
            "target_language_code": "fr",
        }
    )
```

In this example, the source language code is `en-US`, and the target language code is `fr`.

However, it's not always possible for you to know the source language code since users can write in various languages.

To solve this problem, Google has another API to detect the source language.

[Detecting languages (Basic)  |  Cloud Translation  |  Google Cloud](https://cloud.google.com/translate/docs/basic/detecting-language)

```python
def detect_language(text):
    """Detects the text's language."""
    from google.cloud import translate_v2 as translate

    translate_client = translate.Client()

    # Text can also be a sequence of strings, in which case this method
    # will return a sequence of results for each text.
    result = translate_client.detect_language(text)

    print("Text: {}".format(text))
    print("Confidence: {}".format(result["confidence"]))
    print("Language: {}".format(result["language"]))
```

Now, you can combine both of these APIs to translate any language into any target language.

Throughout this blog, we will use the following text as an example for all translations.

> hello dost tum kaise ho kya haal chal

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680406554588/34912df0-db7e-4b84-beda-3769b7adfaab.png align="center")

This piece of text in written in [Hinglish](https://en.wikipedia.org/wiki/Hinglish) (Hindi written in [Latin script](https://en.wikipedia.org/wiki/Latin_script) (instead of the traditional [Devanagari](https://en.wikipedia.org/wiki/Devanagari)), often also mixed with English words or phrases),

Google translate website is able to translate this text as expected.

---

So let's try with the API.

First, let's try to detect the language

```bash
python3 detect_lamguage.py
Text: hello dost tum kaise ho
Confidence: 1
Language: hi-Latn
```

It's working perfectly fine! The Detect Language API is able to detect that the text is in Hindi and written in Latin script.

Now, let's try to run the Translate Text API with the source language set as `hi-Latn` and the target language as `en-US`.

```bash
 "source_language_code": "hi-latn",
  "target_language_code": "en-US",
```

But for the same text, code throws an exception

```bash
	details = "Source language is unsupported."
```

If we change the source language to hi, the Translate API will work, but the results may be very poor.

```bash
Result from API  

Input text: hello dost tum kaise ho kya haal chal
Translated text: hello friend how are you
```

But the google translate website is able to translate text written in Hinglish properly, looks like we are missing something.

---

I tried to find out how Google translate website works, and which API they are using, I attached Charles's web proxy to see the traffic, and all I could see was this URL getting called

[https://translate.google.co.in/\_/TranslateWebserverUi/data/batchexecute](https://translate.google.co.in/_/TranslateWebserverUi/data/batchexecute)?

Nothing useful was found here.

---

I also found out, that if we write proper Hindi, then the translate API returns proper results. Let's try with text written in Hindi(Devanagari)

```bash
Input text: हेलो दोस्त तुम कैसे हो क्या हाल चाल
Translated text: hello friend how are you doing
```

So we need to convert Hinglish to Hindi written in Devanagari, and then use the translate API.

---

### How to convert Hinglish to Hindi

This step is called Transliterate

There is an API from google for this

> [https://pypi.org/project/google-transliteration-api/](https://pypi.org/project/google-transliteration-api/)

But this is a deprecated API [https://developers.google.com/transliterate/v1/getting\_started](https://developers.google.com/transliterate/v1/getting_started)

> The Google Transliterate API has been officially deprecated as of May 26, 2011. It will continue to work as per our [deprecation policy](https://developers.google.com/transliterate/terms).

Deprecation doesn't means that it will be stopped, and you can't use it.

So let's plug in everything, These are the steps to use a working google translate API, that works.

1. Detect language
    
2. Perform transliteration, if required
    
3. Call the google translate API.
    

```bash
python3 translate_tansliterate.py
Please enter something: hello dost tum kaise ho kya haal chal
Text: hello dost tum kaise ho kya haal chal
Confidence: 1
Language: hi-Latn
['हेलो दोस्त तुम कैसे हो क्या हाल चाल', 'हेल्लो दोस्त तुम कैसे हो क्या हाल चाल', 'हेलो दोस्त तुम कैसे हो क्या हाल चल', 'हेल्लो दोस्त तुम कैसे हो क्या हाल चल', 'हेलो दोस्त तुम कैसी हो क्या हाल चाल', 'हेलो दोस्त तुम कइसे हो क्या हाल चाल']
Translated text: hello friend how are you doing
```

Finally, we have a working script that can translate Hinglish into English.

If you enjoyed reading this blog, you can follow me on Twitter to learn something new with me.

[https://twitter.com/nkalra0123](https://twitter.com/nkalra0123)