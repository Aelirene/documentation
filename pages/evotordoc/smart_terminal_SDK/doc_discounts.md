---
title: Скидки
keywords: sample
summary:
sidebar: evotordoc_sidebar
permalink: doc_discounts.html
folder: smart_terminal_SDK
---

### Скидки

Вы можете рассчитывать скидки и передавать их в смарт-терминала. Для этого требуется подписаться на событие `ReceiptDiscountEvent`, которое сообщает о возможности начисления скидки.

#### Подписка на событие

1. Создайте службу, например `MyDiscountService`, которая наследует класс `IntegrationService`. В методе `onCreate` службы, зарегистрируйте процессор `ReceiptDiscountEventProcessor`.
    {% highlight java %}
    public class MyDiscountService extends IntegrationService {

        @Override
        public void onCreate() {
            super.onCreate();

            registerProcessor(
                    new ReceiptDiscountEventProcessor() {
                        @Override
                        public void call(ReceiptDiscountEvent positionsDiscountEvent, Callback callback) {

                        }
                    });
        }
    }
    {% endhighlight %}
2. Объявите службу в манифесте приложения:
    {% highlight xml %}
    <service
            android:name="MyDiscountService"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
                <action android:name="evo.v2.receipt.sell.receiptDiscount" />
            </intent-filter>
    </service>
    {% endhighlight %}

В метод `call` процессора приходит событие `positionsDiscountEvent` и объект для возврата результата `callback`.

Чтобы вернуть результат, используйте метод:
{% highlight java %}
try {callback.onResult(
        new ReceiptDiscountEventResult(
        ReceiptDiscountEventResult.Result.OK,
        new BigDecimal(0.5)
        ).toBundle())
    }
        catch (RemoteException exc) {
                exc.printStackTrace();
            }

{% endhighlight %}
