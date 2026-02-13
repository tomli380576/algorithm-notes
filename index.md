---
icon: home
image: /assets/lmao2.png
---

![](/assets/lmao2.png)

# Algorithm Notes

My notes for 122A, 122B and more.

- [Syntax Reference](./pseudocode-syntax.md)

## Textbooks

- [UIUC textbook](http://algorithms.wtf/) (Free to read)
- [CLRS](https://www.amazon.com/Introduction-Algorithms-3rd-MIT-Press/dp/0262033844)
- [Algorithm Design by Kleinberg](https://www.amazon.com/Algorithm-Design-Jon-Kleinberg/dp/0321295358)


The logo is generated [here](https://bluearchive-logo.esing.dev/).

<span id="updateDate1222">Fetching last updated date...</span>

<script defer>
    window.onload = function() {

        const element = document.getElementById("updateDate1222");
        const url = "https://api.github.com/repos/tomli380576/algorithm-notes/commits";

        fetch(url)
            .then((res) => res.json())
            .then((json) => {
                const dateStr = json[0].commit.author.date;
                element.innerHTML = `Last updated: ${new Date(
                    dateStr
                ).toLocaleDateString()}`;
            });
    }
</script>
