import requests
import bs4
import sys

# print("Hello, you're welcome to the translator. Translator supports:", )
languages = ['arabic', 'german', 'english', 'spanish',
             'french', 'hebrew', 'japanese', 'dutch',
             'polish', 'portuguese', 'romanian', 'russian', 'turkish']


class same_same(Exception):
    pass

def validate_frm_to(argv):
    try:
        frm, to_, word = sys.argv[1:]
    except:
        print("Arguments not enough")
        sys.exit()
    to = []
    try:
        if frm not in languages:
            print(f"Sorry, the program doesn't support {frm}")
            sys.exit()
        if to_ not in [*languages, 'all']:
            print(f"Sorry, the program doesn't support {to_}")
            sys.exit()
            if to_ != 'all':
                print(f"Sorry, the program doesn't support {to_}")
                sys.exit()
        if to_ == frm:
            raise same_same
    except same_same:
        print("Sorry, both the From and To language are the same")
        sys.exit()
    if to_ == 'all':
        to.extend(list(languages))
        del to[to.index(frm)]
    return (frm, to_, word)


frm, to, word = validate_frm_to(sys.argv[1:])

for language in to:
    file = open(f'{word}.txt', 'a')
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) '
                      'Chrome/86.0.4240.75 Safari/537.36'}
    r = requests.get(f'https://context.reverso.net/translation/{frm}-{language}/' + word,
                     headers=headers)
    if r.status_code == 404:
        print("Something wrong with your internet connection")
        sys.exit()
    content = r.content
    soup = bs4.BeautifulSoup(content, 'html.parser')
    translations = []
    try:
        mean = soup.select('#translations-content')[0].select('a')
        example = soup.select('#examples-content')[0].select('.text')
    except IndexError:
        print(f"Sorry, unable to find {word}")
        sys.exit()
    translations.append([a.text.strip() for a in mean][:5])
    translations.append([a.text.strip() for a in example][:10])
    print(f"{language} Translations:")
    print(*translations[0], sep='\n')
    print()
    print(f'{language} Examples:')
    for _ in range(0, len(translations[1]) - 1, 2):
        print(translations[1][_] + ':')
        print(translations[1][_ + 1], '\n')
    print(f"{language} Translations:", file=file)
    print(*translations[0], sep='\n', file=file)
    print(file=file)
    print(f'{language} Examples:', file=file)
    for _ in range(0, len(translations[1]) - 1, 2):
        print(translations[1][_] + ':', file=file)
        print(translations[1][_ + 1], '\n', file=file)
    file.close()
