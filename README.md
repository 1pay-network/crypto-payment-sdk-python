# 1PAY.network - Crypto Payment SDK

Website: [1pay.network](https://1pay.network)

Documents: [1pay.network/documents](https://1pay.network/documents)

Full example of 1PAY.network integration for Python app

Note: **tkinter** is used only to create GUI for python app, you should only care about **webview** library

> Focus on file /main.py

```python
from tkinter import *
import webview


def start():
    # Use a breakpoint in the code line below to debug your script.
    tk = Tk()
    tk.title("Demo 1Pay SDK")
    tk.geometry("480x640")

    frame = Frame(tk, width=480)
    frame.pack()

    button = Button(frame, text="Start", width=25, command=start_payment)
    button.pack()


RECIPIENT = "0x8d70EC40AAd376aa6fD08e4CFD363EaC0AB2c174"
TOKENS = "usdt,usdc,dai"
NETWORKS = "ethereum,optimism,arbitrum,bsc"


def build_payment_url(amount, token, note):
    return ("https://1pay.network/app?"
            + "recipient=" + RECIPIENT
            + "&token=" + TOKENS
            + "&network=" + NETWORKS
            + "&paymentAmount=" + str(amount)
            + "&paymentToken=" + token
            + "&paymentNote=" + note)


class Api:
    def onepay_success(self, response):
        print(response)

    def onepay_failed(self, response):
        print(response)


def on_loaded():
    event_script = (
        "onsuccess=onepaySuccess;onepaySuccess=function(response){onsuccess(response);pywebview.api.onepay_success(response)};"
        "onfailed=onepayFailed;onepayFailed=function(response){onfailed(response);pywebview.api.onepay_failed(response)};")
    webview.windows[0].evaluate_js(event_script)


def start_payment():
    window = webview.create_window(
        "Payment",
        width=480,
        height=640,
        url=build_payment_url(0.1, "usdt", "demo note"),
        js_api=Api()
    )

    window.events.loaded += on_loaded

    webview.start(debug=True, private_mode=False)


# Press the green button in the gutter to run the script.
if __name__ == '__main__':
    start()
    mainloop()
```
