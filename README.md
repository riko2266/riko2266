from machine import Pin, PWM

class MotorController:
    def __init__(self, motor1_pins, motor2_pins):
        # Motor 1 setup
        self.motor1_fwd = Pin(motor1_pins['input1'], Pin.OUT)
        self.motor1_bwd = Pin(motor1_pins['input2'], Pin.OUT)
        self.motor1_en = PWM(Pin(motor1_pins['enable']))
        self.motor1_en.freq(1000)

        # Motor 2 setup
        self.motor2_fwd = Pin(motor2_pins['input1'], Pin.OUT)
        self.motor2_bwd = Pin(motor2_pins['input2'], Pin.OUT)
        self.motor2_en = PWM(Pin(motor2_pins['enable']))
        self.motor2_en.freq(1000)

        self.stop()

    def stop(self):
        self.motor1_fwd.off()
        self.motor1_bwd.off()
        self.motor1_en.duty_u16(0)
        self.motor2_fwd.off()
        self.motor2_bwd.off()
        self.motor2_en.duty_u16(0)

    def forward(self, speed=30000):
        self.motor1_fwd.on()
        self.motor1_bwd.off()
        self.motor1_en.duty_u16(speed)
        self.motor2_fwd.on()
        self.motor2_bwd.off()
        self.motor2_en.duty_u16(speed)

    def backward(self, speed=30000):
        self.motor1_fwd.off()
        self.motor1_bwd.on()
        self.motor1_en.duty_u16(speed)
        self.motor2_fwd.off()
        self.motor2_bwd.on()
        self.motor2_en.duty_u16(speed)

    def left(self, speed=30000):
        self.motor1_fwd.off()
        self.motor1_bwd.on()
        self.motor1_en.duty_u16(speed)
        self.motor2_fwd.on()
        self.motor2_bwd.off()
        self.motor2_en.duty_u16(speed)

    def right(self, speed=30000):
        self.motor1_fwd.on()
        self.motor1_bwd.off()
        self.motor1_en.duty_u16(speed)
        self.motor2_fwd.off()
        self.motor2_bwd.on()
        self.motor2_en.duty_u16(speed)

    def forward_left(self, speed=30000):
        self.motor1_fwd.on()
        self.motor1_bwd.off()
        self.motor1_en.duty_u16(speed // 2)
        self.motor2_fwd.on()
        self.motor2_bwd.off()
        self.motor2_en.duty_u16(speed)

    def forward_right(self, speed=30000):
        self.motor1_fwd.on()
        self.motor1_bwd.off()
        self.motor1_en.duty_u16(speed)
        self.motor2_fwd.on()
        self.motor2_bwd.off()
        self.motor2_en.duty_u16(speed // 2)

    def backward_left(self, speed=30000):
        self.motor1_fwd.off()
        self.motor1_bwd.on()
        self.motor1_en.duty_u16(speed // 2)
        self.motor2_fwd.off()
        self.motor2_bwd.on()
        self.motor2_en.duty_u16(speed)

    def backward_right(self, speed=30000):
        self.motor1_fwd.off()
        self.motor1_bwd.on()
        self.motor1_en.duty_u16(speed)
        self.motor2_fwd.off()
        self.motor2_bwd.on()
        self.motor2_en.duty_u16(speed // 2)

    def rotate_left(self, speed=30000):
        self.motor1_fwd.off()
        self.motor1_bwd.on()
        self.motor1_en.duty_u16(speed)
        self.motor2_fwd.on()
        self.motor2_bwd.off()
        self.motor2_en.duty_u16(speed)

    def rotate_right(self, speed=30000):
        self.motor1_fwd.on()
        self.motor1_bwd.off()
        self.motor1_en.duty_u16(speed)
        self.motor2_fwd.off()
        self.motor2_bwd.on()
        self.motor2_en.duty_u16(speed)

