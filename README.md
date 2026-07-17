# CRMRP-RUSSIA
OFFICIAL SITE
import random

# Лимит денег
MAX_MONEY = 8000

# Статьи КоАП (примерные)
COAP_ARTICLES = {
    "12.8": {"name": "Управление ТС в состоянии опьянения", "fine": 1500},
    "12.9": {"name": "Превышение скорости", "fine": 500},
    "20.1": {"name": "Мелкое хулиганство", "fine": 300},
    "6.3": {"name": "Нарушение санитарных норм", "fine": 400},
    "7.27": {"name": "Мелкое хищение", "fine": 200},
}

class Player:
    def __init__(self, name, money=1000):
        self.name = name
        self.money = min(money, MAX_MONEY)
        self.history = []  # история штрафов

    def pay_fine(self, article_code):
        article = COAP_ARTICLES.get(article_code)
        if not article:
            return False, "Неизвестная статья."

        fine = article["fine"]
        if self.money < fine:
            return False, f"Недостаточно средств. На счету: {self.money} €, штраф: {fine} €."

        self.money -= fine
        entry = {
            "article": article_code,
            "name": article["name"],
            "fine": fine,
            "remaining": self.money
        }
        self.history.append(entry)
        return True, f"Штраф по ст. {article_code} ({article['name']}) выписан. Сумма: {fine} €. Остаток: {self.money} €."

    def add_money(self, amount):
        self.money = min(self.money + amount, MAX_MONEY)


def detect_violation(text: str):
    """
    Очень простая эвристика для RP-детектирования нарушений.
    В реальной игре лучше использовать ML/LLM или ручной разбор сценариев.
    """
    text = text.lower()
    if any(x in text for x in ["пьян", "опьянен", "выпил", "алкоголь"]):
        return "12.8"
    if "скорость" in text or "превысил" in text:
        return "12.9"
    if any(x in text for x in ["хулиган", "драка", "ругался", "кричал"]):
        return "20.1"
    if "украл" in text or "стащил" in text:
        return "7.27"
    return None


def play_rp_game():
    print("=== RP-игра: Полицейский патруль ===")
    name = input("Введите имя персонажа: ").strip() or "Игрок"
    player = Player(name)

    print(f"Добро пожаловать, {player.name}. Ваш баланс: {player.money} € (макс. {MAX_MONEY} €).")
    print("Описывайте свои действия. Если вы нарушаете правила, вам выпишут штраф по статье КоАП.")
    print("Примеры действий: «я ехал быстро», «я выпил», «я подрался».")
    print("Команды: /баланс, /история, /выход\n")

    while True:
        action = input("> ").strip()
        if action == "/выход":
            print("Игра окончена.")
            break
        elif action == "/баланс":
            print(f"Ваш баланс: {player.money} €.")
            continue
        elif action == "/история":
            if not player.history:
                print("Штрафов нет.")
            else:
                for i, h in enumerate(player.history, 1):
                    print(f"{i}. Ст. {h['article']} ({h['name']}): {h['fine']} € → остаток {h['remaining']} €")
            continue

        # Детектируем нарушение
        violation = detect_violation(action)
        if violation:
            ok, msg = player.pay_fine(violation)
            if ok:
                print(f"[ПОЛИЦИЯ] Нарушение зафиксировано! {msg}")
            else:
                print(f"[ПОЛИЦИЯ] {msg}")
        else:
            # Нейтральная реакция на действие без нарушения
            responses = [
                "Патруль продолжает наблюдение.",
                "Ничего подозрительного не замечено.",
                "Продолжайте, но не нарушайте.",
                "Зафиксировано ваше действие, нарушений нет.",
            ]
            print(random.choice(responses))


if __name__ == "__main__":
    play_rp_game()
