   import re
   import sys

   def scan_file(filename):
       print(f"[*] Scanning {filename}...")

       vulnerabilities = {
           "SQL Injection": r"execute\(.*%.*\)|query\(.*\$_",
           "XSS": r"echo\s*\$_|print\(.*\$_",
           "Hardcoded Password": r"password\s*=\s*['\"].+['\"]",
           "Insecure Function": r"eval\(|exec\("
       }

       try:
           with open(filename, 'r', encoding='utf-8') as f:
               code = f.read()
               found = False
               for vuln_name, pattern in vulnerabilities.items():
                   if re.search(pattern, code, re.IGNORECASE):
                       print(f"[!] Potential {vuln_name} found")
                       found = True
               if not found:
                   print("[+] No common vulnerabilities found")
       except FileNotFoundError:
           print("[-] File not found")

   if __name__ == "__main__":
       if len(sys.argv) > 1:
           scan_file(sys.argv[1])
       else:
           print("Usage: python scanner.py test.php")
