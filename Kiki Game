import random

class Human:
    def __init__(self, name, level, hp, mp, intelligence, agility, strength):
        self._name = name
        self._level = level
        self._hp = hp
        self._max_hp = hp
        self._mp = mp
        self._max_mp = mp
        self._strength = strength
        self._agility = agility
        self._intelligence = intelligence
        self._effects = []

    @property
    def hp(self):
        return self._hp

    @hp.setter
    def hp(self, value):
        self._hp = max(0, min(value, self._max_hp))

    @property
    def mp(self):
        return self._mp

    @mp.setter
    def mp(self, value):
        self._mp = max(0, min(value, self._max_mp))

    @property
    def is_alive(self):
        return self._hp > 0

    @property
    def initiative(self):
        return self._agility + random.randint(1, 10)

    def take_damage(self, damage):
        actual_damage = max(0, damage)
        self.hp = self._hp - actual_damage  # ИСПРАВЛЕНО: используем сеттер правильно
        print(f"{self._name} получил {actual_damage} урона. Осталось HP: {self.hp}")
        return actual_damage

    def attack(self, target):
        base_damage = self._strength
        crit_chance = self._agility / 100
        is_critical = random.random() < crit_chance

        damage = base_damage * 2 if is_critical else base_damage
        crit_text = " КРИТИЧЕСКИЙ УРОН!" if is_critical else ""

        print(f"{self._name} атакует {target._name} и наносит {damage} урона.{crit_text}")
        target.take_damage(damage)

    def add_effect(self, effect):
        self._effects.append(effect)
        print(f"{self._name} получает эффект: {effect.name}")

    def process_effects(self):
        for effect in self._effects[:]:
            effect.apply(self)
            effect.duration -= 1
            if effect.duration <= 0:
                self._effects.remove(effect)
                print(f"Эффект {effect.name} не влияет на {self._name}")

    def __str__(self):
        status = "ЖИВ" if self.is_alive else "МЕРТВ"
        return f"{self._name} (Ур.{self._level}) HP: {self._hp}/{self._max_hp} MP: {self._mp}/{self._max_mp} [{status}]"

    def __repr__(self):
        return f"Human('{self._name}', {self._level}, {self._hp}, {self._mp}, {self._strength}, {self._agility}, {self._intelligence})"

    def to_dict(self):
        return {
            'name': self._name,
            'level': self._level,
            'hp': self._hp,
            'max_hp': self._max_hp,
            'mp': self._mp,
            'max_mp': self._max_mp,
            'strength': self._strength,
            'agility': self._agility,
            'intelligence': self._intelligence,
            'effects': [effect.to_dict() for effect in self._effects]
        }


class Warrior(Human):
    def __init__(self, name, level=1):
        super().__init__(name, level, hp=120, mp=30, strength=15, agility=8, intelligence=5)

    def attack(self, target):
        base_damage = self._strength + 5
        print(f"{self._name} атакует огненным мечом {target._name}!")
        damage = base_damage
        target.take_damage(damage)

    def power_strike(self, target):
        if self._mp >= 15:
            self.mp = self._mp - 15
            damage = self._strength * 2
            print(f"{self._name} использует МОЩНЫЙ УДАР по {target._name}!")
            target.take_damage(damage)
            return True
        else:
            print(f"Недостаточно маны для использования Мощного удара!")
            return False


class Mage(Human):
    def __init__(self, name, level=1):
        super().__init__(name, level, hp=80, mp=100, strength=5, agility=6, intelligence=18)

    def attack(self, target):
        damage = self._intelligence
        print(f"{self._name} бросает магическую стрелу в {target._name}!")
        target.take_damage(damage)


class Healer(Human):
    def __init__(self, name, level=1):
        super().__init__(name, level, hp=90, mp=80, strength=6, agility=7, intelligence=14)

    def attack(self, target):
        damage = max(3, self._strength - 2)
        print(f"{self._name} атакует посохом {target._name}!")
        target.take_damage(damage)

    def heal(self, target):
        if self._mp >= 20:
            self.mp = self._mp - 20
            heal_amount = self._intelligence + random.randint(5, 10)
            old_hp = target.hp
            target.hp += heal_amount
            actual_heal = target.hp - old_hp
            print(f"{self._name} исцеляет {target._name} на {actual_heal} HP!")
            return True
        else:
            print(f"Недостаточно маны для исцеления!")
            return False

    def regeneration(self, target):
        if self._mp >= 30:
            self.mp = self._mp - 30
            print(f"{self._name} накладывает Регенерацию на {target._name}!")
            return True
        else:
            print(f"Недостаточно маны для Регенерации!")
            return False


class Boss(Human):
    def __init__(self, name, level=5):
        super().__init__(name, level, hp=300, mp=150, strength=20, agility=10, intelligence=12)

    def attack(self, target):
        damage = self._strength + 10
        print(f"Босс {self._name} атакует {target._name} с силой {damage}!")
        target.take_damage(damage)

    def take_damage(self, damage):
        super().take_damage(damage)
        if self.hp < 100:
            print(f"Босс {self._name} в ярости! У него мало HP!")


class Battle:
    def __init__(self):
        self.participants = []

    def add_participant(self, participant):
        self.participants.append(participant)
        print(f"{participant._name} присоединился к битве")

    def simple_attack(self):
        if len(self.participants) < 2:
            print("Нужно как минимум 2 участника для боя!")
            return

        attacker = self.participants[0]
        target = self.participants[1]

        print(f"\n=== ПРОСТАЯ АТАКА ===")
        print(f"Атакующий: {attacker._name}")
        print(f"Цель: {target._name}")

        attacker.attack(target)

        print(f"Результат: {target._name} теперь имеет {target.hp} HP")

    def random_attack(self):
        alive = [p for p in self.participants if p.is_alive]

        if len(alive) < 2:
            print("Недостаточно живых участников для атаки!")
            return

        attacker = random.choice(alive)
        possible_targets = [p for p in alive if p != attacker]
        target = random.choice(possible_targets)

        print(f"\n=== СЛУЧАЙНАЯ АТАКА ===")
        print(f"Атакующий: {attacker._name}")
        print(f"Цель: {target._name}")

        attacker.attack(target)

    def display_all(self):
        print(f"\n=== УЧАСТНИКИ БОЯ ===")
        for participant in self.participants:
            print(participant)
