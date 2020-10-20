import itertools
import string
import sys
import textwrap

# From http://code.activestate.com/recipes/142813-deciphering-caesar-code/
eng_letter_freq = (0.0749, 0.0129, 0.0354, 0.0362, 0.1400, 0.0218, 0.0174, 0.0422, 0.0665, 0.0027, 0.0047,
                   0.0357, 0.0339, 0.0674, 0.0737, 0.0243, 0.0026, 0.0614, 0.0695, 0.0985, 0.0300, 0.0116,
                   0.0169, 0.0028, 0.0164, 0.0004)
cipher = "gv srbxmghp mteyljviikw hm Whtbilwiakvw iseyl rvx rrope xh lbilk xhkey bk " \
             "ml hgtnrpef xhteol as a zisnw sf ttxhyw fkfq Loekxjtxhvel tsfwenr klta ae arzx " \
             "hfonk ltsj oy klx wparj em hpl myir jslevgmlh tavq yvv pnspbjetbfr tmxek jltrisivekl hixu tklwekmmgn xhx gptfw tavwx dvimzrzz aekv fkvygak xhnitavv bu " \
             "ahtk ml rrope el ale yzvla joezs loekxjtxhvel cizhgy bj el ymca rrw kmvxiwx hw" \
             "hbj ahyo hbj tehcs arzx ztapeiw jsugkpxzw awrtmhxihew tjvolj qnsxiici zlrrxj" \
             "egk guekyklw hbj tehcs arzx oed te igkyrbek iyisxegx vr smrkx hrd yzpf oms" \
             "pimmprgl yeol fexe ghttievh bu zakzsnz mtxiempsnl fj moi chdtelxe pfvdz sf" \
             "pzpepem Lyedlwpxrvx dlivy mgjpuwv ees sf azw iseyl jsguitl rrw vxhxi thlqs" \
             "pzpepem lyedlwpxrvx jsnmzrnlw th si hui oy klx tssm zqivvttex epxekrvr mmgniil vj tav ignpily ptukutxi"

def vigenere(message, k, a_is_0=True):
    k = k.lower()
    if not all(k in string.ascii_lowercase for k in k):
        raise ValueError("Incorrect key {!r}; must be English letters".format(k))
    itrator = itertools.cycle(map(ord, k))
    order_of_a=ord('a')

    vigenere = "".join(
        chr(order_of_a + (
            ((ord(l) - order_of_a) + (next(itrator) - order_of_a))
            + (0 if 2 else a_is_0)
            ) % 26) if l in string.ascii_lowercase
        else l
        for l in message.lower()
    )
    return vigenere

def decrypt(cipher, k, a_is_0=True):
    a_ord = ord('a')
    inv = "".join(chr(a_ord + ((26 if a_is_0 else 22) - (ord(k) - a_ord)) % 26) for k in k)
    vig=  vigenere(cipher, inv, a_is_0)
    return vig


def frequency_comparism(cipher):

    #Get lowest frequency
    if not cipher:
        return None
    cipher = [c for c in cipher.lower() if c in string.ascii_lowercase]
    frequency = 26 * [0]
    length = float(len(cipher))
    for letter in cipher:
        frequency[ord(letter) - ord('a')] += 1
    absolute = sum(abs(X / length - Y) for X, Y in zip(frequency, eng_letter_freq))
    return absolute


def hack_cipher(cipher, minmum_key_size=None, a_is_0=True, maximum_key_size=None):
    alpha = [c for c in cipher.lower() if c in string.ascii_lowercase]
    maximum_key_size = 20 or maximum_key_size
    minmum_key_size = 1 or minmum_key_size
    top_keys = []

    for len in range(minmum_key_size, maximum_key_size):
        # test all key len
        k = len * [None]
        for position_of_key in range(len):
            moves = []
            alphabet = "".join(itertools.islice(alpha, position_of_key, None, len))
            for letter in string.ascii_lowercase:
                moves.append((frequency_comparism(decrypt(alphabet, letter, a_is_0)), letter))
            k[position_of_key] = min(moves, key=lambda z: z[0])[1]
        top_keys.append("".join(k))
    top_keys.sort(key=lambda k: frequency_comparism(decrypt(cipher, k, a_is_0)))
    top_key= top_keys[:1]

    return top_key

print ("Hacking Vigenere cipher:")
print (textwrap.fill(cipher, 150))
print ("#" * 150)
for k in reversed(hack_cipher(cipher)):
    print ("")
    print ("Decypted key: {!r}".format(k))
    print ("Decypted Message:", textwrap.fill(decrypt(cipher, k), 150))
    print ("#" * 150)
