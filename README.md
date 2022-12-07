import subprocess
from unittest import result


def wifikamui(NET, WAN, SHW, PRS):
    data = subprocess.check_output([NET, WAN, SHW, PRS]).decode('utf-8', errors="backslashreplace").split('\n')
    profile = [wifi.split(":")[1][1:-1] for wifi in data if "All User Profile" in wifi]

    for send in profile:
        results = subprocess.check_output([NET, WAN, SHW, PRS, send, 'key=clear']).decode('utf-8', errors="backslashreplace").split('\n')
        results = [wifi_pass.split(":")[1][1:-1] for wifi_pass in results if "Key Content" in wifi_pass]
        try:
            print("{:<3} :  {:<}".format(send, results[0]))
        except IndexError:
            print("{:<3} :  {:<}".format(wifi_pass, "None"))

SHOW = wifikamui('netsh', 'wlan', 'show', 'profiles')
