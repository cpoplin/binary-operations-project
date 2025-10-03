

class SignMagnitude16BitBinary(object):
    def __init__(self, num):
        self.bits = []
        if num >= 0:
            self.bits.append(0)
        else:
            self.bits.append(1)
        num = abs(num)
        while num > 0:
            self.bits.insert(1, num % 2)
            num = num // 2
        while len(self.bits) < 16:
            self.bits.insert(1, 0)
    def __add__(self, other):
        retval = SignMagnitude16BitBinary(0)
        carry = 0
        if (self.bits[0] == 0 and other.bits[0] == 0) or (self.bits[0] == 1 and other.bits[0] == 1):
            for i in range(len(self.bits) - 1, 0, -1):
                if self.bits[i] + other.bits[i] + carry== 2:
                    carry = 1
                    retval.bits[i] = 0
                elif self.bits[i] + other.bits[i] + carry == 3:
                    carry = 1
                    retval.bits[i] = 1
                else:
                    retval.bits[i] = self.bits[i] + other.bits[i] + carry
                    carry = 0
            if carry == 1:
                raise Exception("Overflow")
            return retval
        elif self.abs_greater(other):
            if self.bits[0] == 0:
                temp = SignMagnitude16BitBinary(0)
                temp.bits = self.bits[:]
                retval = SignMagnitude16BitBinary._subtract(self, temp)
                retval.bits[0] = 0
                return retval
            else:
                retval = SignMagnitude16BitBinary._subtract(self, other)
                retval.bits[0] = 1
                return retval
        elif other.abs_greater(self):
            if other.bits[0] == 0:
                retval = SignMagnitude16BitBinary._subtract(other, self)
                retval.bits[0] = 0
                return retval
            else:
                retval = SignMagnitude16BitBinary._subtract(other, self)
                retval.bits[0] = 1
                return retval
        else:
            return SignMagnitude16BitBinary(0)
    def __sub__(self,other):
        temp = SignMagnitude16BitBinary(0)
        temp.bits = other.bits[:]
        temp.bits[0] = 1 if temp.bits[0] == 0 else 0
        # print(temp.bits, temp.get_decimal())
        return self + temp
    def __eq__(self, other):
        return self.bits == other.bits
    def __lt__(self,other):
        if self.bits[0] != other.bits[0]:
            return self.bits[0] > other.bits[0]
        for i in range(1, len(self.bits)):
            if self.bits[i] < other.bits[i]:
                return True
            elif self.bits[i] > other.bits[i]:
                return False
    def __gt__(self,other):
        return not self.__lt__(other)
    def __le__(self,other):
        return self.__lt__(other) or self.__eq__(other)
    def __ge__(self,other):
        return self.__gt__(other) or self.__eq__(other)

    def _subtract(one, two):
        retval = SignMagnitude16BitBinary(0)
        for i in range(len(one.bits)-1, 0,-1):
            if one.bits[i] - two.bits[i] < 0 and i != 1:
                one.bits[i] += 2
                one.bits[i-1] -= 1
            retval.bits[i] = one.bits[i] - two.bits[i]
        if retval.bits[1] < 0:
            # print(retval.bits)
            raise Exception("Underflow")
        return retval
    def get_decimal(self):
        retval = 0
        j = 0
        for i in range(len(self.bits) - 1, 0, -1):
            if self.bits[i] == 1:
                retval += 2**j
            j += 1
        if self.bits[0] == 1:
            retval *= -1
        return retval
    def abs_greater(self, other):
        for i in range(1, len(self.bits)):
            if self.bits[i] < other.bits[i]:
                return False
            elif self.bits[i] > other.bits[i]:
                return True


# s = SignMagnitude16BitBinary(-80)
# z = SignMagnitude16BitBinary(60)
# # print(s.bits)
# # print(z.bits)
# # x = s - z
# # print(x.get_decimal())
# print(s.abs_greater(z))

# num7 = 40
# num8 = -60
# s = SignMagnitude16BitBinary(num7)
# z = SignMagnitude16BitBinary(num8)
# x = s - z
# print(f"The result of the binary subtraction of {num7} and {num8} is {x.bits} in binary and {x.get_decimal()} in decimal \n")

# s = SignMagnitude16BitBinary(-60)
# z = SignMagnitude16BitBinary(80)
# x = s + z
# print(s.bits, "\n", z.bits)
# print(x.get_decimal(), x.bits)