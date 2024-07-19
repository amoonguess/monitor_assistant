import time
import pyautogui
import datetime


class MonitorAssistant:
    def __init__(self):
        self.x_max, self.y_max = pyautogui.size()
        self.x_vacant = None
        self.y_vacant = None
        self.start = None
        self.end = None
        self.movement = 0
        self.state = True

    def get_vacant_position(self, vacant_position=(0, 0)):
        if vacant_position == (0, 0):
            self.x_vacant = self.x_max * 0.75
            self.y_vacant = self.y_max - 2
        else:
            self.x_vacant, self.y_vacant = vacant_position

    def check_activity(self, mode='m', time_range=60, freq=180, max_dist=500):
        """
        mode='m', check the activity by the movement of cursor, using Manhattan Distance
        """
        self.start = datetime.datetime.now()

        def check_move():
            self.movement = 0
            position = []
            for _ in range(freq):
                current_pos = pyautogui.position()
                position.append((current_pos.x, current_pos.y))
                time.sleep(time_range / freq)

            distance = [abs(i[0] - j[0]) + abs(i[1] - j[1]) for i, j in zip(position[1:], position[:-1])]
            self.movement = sum(distance)

            if self.movement > max_dist:
                self.state = False

        if mode == 'm':
            self.state = True
            check_move()

        else:
            raise NotImplementedError

        self.end = datetime.datetime.now()

    def click(self):
        if self.state:
            pyautogui.click(self.x_vacant, self.y_vacant)

    def __repr__(self):
        if self.state:
            return f'During {self.start} to {self.end} --> No action'
        elif not self.state:
            return f'During {self.start} to {self.end} --> action detected!'


if __name__ == '__main__':
    assistant = MonitorAssistant()
    assistant.get_vacant_position()
    while True:
        assistant.check_activity()
        assistant.click()
        print(repr(assistant))
