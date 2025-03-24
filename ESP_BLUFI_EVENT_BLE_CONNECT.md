[[IoT]]

`ESP_BLUFI_EVENT_BLE_CONNECT` — это событие, связанное с **BLUFI** (Bluetooth Low Energy Wi-Fi Configuration), которое возникает, когда устройство ESP32 успешно устанавливает BLE-соединение с клиентом (например, смартфоном). Это событие используется в рамках **ESP-BLUFI**, компонента ESP-IDF, который позволяет настраивать Wi-Fi на устройстве ESP32 через Bluetooth Low Energy (BLE).

---

### Что такое BLUFI?
**BLUFI** — это протокол, разработанный Espressif, который позволяет передавать конфигурацию Wi-Fi (SSID и пароль) на устройство ESP32 через BLE. Это особенно полезно для устройств, у которых нет других интерфейсов для ввода данных (например, кнопок или дисплеев).

---

### Событие `ESP_BLUFI_EVENT_BLE_CONNECT`
- **Тип события**: Это событие генерируется, когда устройство ESP32 успешно устанавливает BLE-соединение с клиентом.
- **Когда возникает**: После того, как клиент (например, приложение на смартфоне) подключается к устройству ESP32 через BLE.
- **Данные события**: Обычно это событие не содержит дополнительных данных.

---

### Пример использования `ESP_BLUFI_EVENT_BLE_CONNECT`

```c
#include "esp_blufi_api.h"
#include "esp_log.h"

static const char *TAG = "BLUFI_EXAMPLE";

// Обработчик событий BLUFI
void blufi_event_handler(void *event_handler_arg, esp_event_base_t event_base, int32_t event_id, void *event_data) {
    switch (event_id) {
        case ESP_BLUFI_EVENT_BLE_CONNECT:
            ESP_LOGI(TAG, "BLE connected");
            break;

        case ESP_BLUFI_EVENT_BLE_DISCONNECT:
            ESP_LOGI(TAG, "BLE disconnected");
            break;

        case ESP_BLUFI_EVENT_RECV_SLAVE_DISCONNECT_BLE:
            ESP_LOGI(TAG, "Received disconnect command from client");
            break;

        case ESP_BLUFI_EVENT_RECV_STA_SSID:
            ESP_LOGI(TAG, "Received SSID: %s", (char *)event_data);
            break;

        case ESP_BLUFI_EVENT_RECV_STA_PASSWD:
            ESP_LOGI(TAG, "Received password: %s", (char *)event_data);
            break;

        case ESP_BLUFI_EVENT_RECV_SOFTAP_SSID:
            ESP_LOGI(TAG, "Received SoftAP SSID: %s", (char *)event_data);
            break;

        case ESP_BLUFI_EVENT_RECV_SOFTAP_PASSWD:
            ESP_LOGI(TAG, "Received SoftAP password: %s", (char *)event_data);
            break;

        default:
            break;
    }
}

void app_main() {
    // Инициализация BLUFI
    esp_blufi_callbacks_t blufi_callbacks = {
        .event_cb = blufi_event_handler
    };
    esp_blufi_register_callbacks(&blufi_callbacks);

    // Запуск BLUFI
    esp_blufi_profile_init();

    ESP_LOGI(TAG, "BLUFI initialized, waiting for BLE connection...");
}
```

---

### Как это работает:
1. **Инициализация BLUFI**:
   - Регистрируются callback-функции для обработки событий BLUFI.
   - Инициализируется профиль BLUFI.

2. **Ожидание подключения**:
   - Устройство ESP32 начинает рекламироваться через BLE, ожидая подключения клиента.

3. **Обработка события `ESP_BLUFI_EVENT_BLE_CONNECT`**:
   - Когда клиент подключается, генерируется событие `ESP_BLUFI_EVENT_BLE_CONNECT`.
   - В обработчике событий можно выполнить необходимые действия, например, начать передачу данных.

4. **Получение конфигурации Wi-Fi**:
   - После подключения клиент может отправить SSID и пароль Wi-Fi через BLE, что вызовет соответствующие события (например, `ESP_BLUFI_EVENT_RECV_STA_SSID` и `ESP_BLUFI_EVENT_RECV_STA_PASSWD`).

---

### Другие события BLUFI
- `ESP_BLUFI_EVENT_BLE_DISCONNECT`: Событие отключения BLE-соединения.
- `ESP_BLUFI_EVENT_RECV_STA_SSID`: Получение SSID Wi-Fi сети.
- `ESP_BLUFI_EVENT_RECV_STA_PASSWD`: Получение пароля Wi-Fi сети.
- `ESP_BLUFI_EVENT_RECV_SOFTAP_SSID`: Получение SSID для SoftAP.
- `ESP_BLUFI_EVENT_RECV_SOFTAP_PASSWD`: Получение пароля для SoftAP.

---

### Где использовать `ESP_BLUFI_EVENT_BLE_CONNECT`?
- **Настройка Wi-Fi через BLE**: Когда устройство ESP32 не имеет других интерфейсов для ввода данных.
- **IoT-устройства**: Для упрощения настройки устройств в сети.
- **Прототипирование**: Быстрая настройка Wi-Fi во время разработки.

---

### Итог:
`ESP_BLUFI_EVENT_BLE_CONNECT` — это важное событие в протоколе BLUFI, которое сигнализирует об успешном подключении клиента через BLE. Оно используется для начала процесса передачи конфигурации Wi-Fi на устройство ESP32. Этот механизм упрощает настройку устройств, особенно в IoT-сценариях.