# Онлайн Пінг-Понг

> Це шаблон для створення власної онлайн-гри **Пінг-Понг** на двох гравців. Гра вже працює через локальну мережу — залишилось зробити її красивою! 

## 🔧 Що вже зроблено:

- ✅ Готовий сервер і клієнт (гравець)
- ✅ Фізика м’яча та платформи
- ✅ Перемога при рахунку 10
- ✅ Очікування другого гравця
- ✅ Події зіткнення (для звуків)

## Як запустити проєкт

1. **Завантаж проєкт**
   - Можеш клонувати або завантажити ZIP:
     ```
     git clone https://github.com/KravaAO/ping-pong.git
     ```

2. **Увімкни сервер**
   - Відкрий `server.py` і запусти його:
     ```
     python server.py
     ```

3. **Запусти клієнта (2 рази)**
   - Відкрий два вікна (на одному або різних комп’ютерах з допомогою викладача) і запусти `client.py`:
     ```
     python client.py
     ```

> ⚠️ Усі файли мають бути у тій самій папці. Сервер запускається **лише один раз**, а `client.py` — для кожного гравця.

## Як це працює?

### Сервер (`server.py`)
- Очікує **двох гравців**
- Веде гру: рухає м’яч, перевіряє зіткнення, надсилає **стан гри**
- Приймає команди `UP`, `DOWN` від гравців

### Клієнт (`client.py`)
- Підключається до сервера 
- Відображає гру, платформи, м’яч, рахунок
- Відправляє натискання клавіш (`W` / `S`) на сервер


> 🏆 Найкреативніші проєкти отримують додаткові логіки!
>https://opengameart.org/content/dull-explosion
> https://opengameart.org/content/elevato
> https://opengameart.org/content/knife-sharpening-slice-2

Успіхів! Нехай твоя гра буде найкращою!

launcher

from pygame import *

WIDTH, HEIGHT = 500, 500


def launcher():
    init()
    screen = display.set_mode((WIDTH, HEIGHT))
    display.set_caption("Pong Launcher")
    clock = time.Clock()

    font_title = font.Font(None, 56)
    font_btn = font.Font(None, 42)
    font_input = font.Font(None, 36)

    menu_mode = "main"
    input_text = ""
    active_input = False
    ball_color = (255, 255, 255)

    while True:
        screen.fill((20, 20, 30))

        title = font_title.render("PING-PONG", True, (0, 200, 255))
        screen.blit(title, title.get_rect(center=(WIDTH // 2, 40)))

        if menu_mode == "main":
            play_btn = Rect(100, 100, 300, 70)
            settings_btn = Rect(100, 200, 300, 70)
            exit_btn = Rect(100, 300, 300, 70)
            input_box = Rect(100, 400, 300, 50)

            draw.rect(screen, (0, 180, 0), play_btn, border_radius=12)
            draw.rect(screen, (180, 180, 0), settings_btn, border_radius=12)
            draw.rect(screen, (180, 0, 0), exit_btn, border_radius=12)
            draw.rect(screen, (255, 255, 255), input_box, 2, border_radius=8)

            screen.blit(font_btn.render("Почати", True, (0, 0, 0)),
                        font_btn.render("Почати", True, (0, 0, 0)).get_rect(center=play_btn.center))
            screen.blit(font_btn.render("Редагування", True, (0, 0, 0)),
                        font_btn.render("Редагування", True, (0, 0, 0)).get_rect(center=settings_btn.center))
            screen.blit(font_btn.render("Вихід", True, (0, 0, 0)),
                        font_btn.render("Вихід", True, (0, 0, 0)).get_rect(center=exit_btn.center))

            screen.blit(font_input.render("Ім'я гравця:", True, (255, 255, 255)), (100, 370))
            screen.blit(font_input.render(input_text, True, (255, 255, 255)), (110, 410))

        elif menu_mode == "settings":
            pink_btn = Rect(100, 150, 300, 60)
            yellow_btn = Rect(100, 230, 300, 60)
            blue_btn = Rect(100, 310, 300, 60)
            back_btn = Rect(100, 390, 300, 60)

            draw.rect(screen, (255, 0, 0), pink_btn, border_radius=12)
            draw.rect(screen, (0, 255, 0), yellow_btn, border_radius=12)
            draw.rect(screen, (0, 0, 255), blue_btn, border_radius=12)
            draw.rect(screen, (150, 150, 150), back_btn, border_radius=12)

            screen.blit(font_btn.render("Рожевий", True, (0, 0, 0)),
                        font_btn.render("Рожевий", True, (0, 0, 0)).get_rect(center=pink_btn.center))
            screen.blit(font_btn.render("Жовтий", True, (0, 0, 0)),
                        font_btn.render("Жовтий", True, (0, 0, 0)).get_rect(center=yellow_btn.center))
            screen.blit(font_btn.render("Синій", True, (255, 255, 255)),
                        font_btn.render("Синій", True, (255, 255, 255)).get_rect(center=blue_btn.center))
            screen.blit(font_btn.render("Назад", True, (0, 0, 0)),
                        font_btn.render("Назад", True, (0, 0, 0)).get_rect(center=back_btn.center))

        for e in event.get():
            if e.type == QUIT:
                quit()
            if e.type == MOUSEBUTTONDOWN:
                if menu_mode == "main":
                    if play_btn.collidepoint(e.pos):
                        return input_text, ball_color
                    if settings_btn.collidepoint(e.pos):
                        menu_mode = "settings"
                    if exit_btn.collidepoint(e.pos):
                        quit()
                    active_input = input_box.collidepoint(e.pos)

                elif menu_mode == "settings":
                    if pink_btn.collidepoint(e.pos):
                        ball_color = (255, 0, 0)
                    if yellow_btn.collidepoint(e.pos):
                        ball_color = (0, 255, 0)
                    if blue_btn.collidepoint(e.pos):
                        ball_color = (0, 0, 255)
                    if back_btn.collidepoint(e.pos):
                        menu_mode = "main"
                    if e.type == KEYDOWN and active_input:
                        if e.key == K_BACKSPACE:
                            input_text = input_text[:-1]
                        elif len(input_text) < 12:
                            input_text += e.unicode

                    display.update()
                    clock.tick(60)
