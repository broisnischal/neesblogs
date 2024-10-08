---
title: " ✨ create the bookmark parser"
description: "Lorem ipsum dolor sit amet"
pubDate: "Jul 08 2022"
heroImage: "https://nischal-dahal.com.np/bookmark.png"
---

If you haven't had looked what the exported bookmark file look like, here i show
how it looks like.

![image](https://nischal-dahal.com.np/bookmark.png)

Ok, I guess now you will say what gibberish it is. But you also had the idea to
create the website, or some function, let me say a service worker, that syncs
your bookmarks with server, and publish that message to other browsers, to sync
all bookmarks.

---

Today, we are going to build the bookmark parser, first we are going to require
a package called [cheerio](https://cheerio.js.org) to parse the html file.

I am using typescript, you can use whichever language you want, just be sure you
can parse a html file. maybe you can build a htmlparser from **C** ?

```tsx
interface Bookmark {
  title: string;
  url: string | undefined;
}
```

Here is a interface of Bookmark, i am not gonna include the icon, in today's
example, but feel free to include as per your need. You can find **ICON** in the
bookmark, data and ADD_DATE also in the file.

```tsx
export const parseBookmarks = (fileContent: string): Bookmark[] => {
  const $ = cheerio.load(fileContent);

  const bookmarks: Bookmark[] = [];
  $("a").each((index, element) => {
    const title = $(element).text();
    const url = $(element).attr("href");
    bookmarks.push({
      title,
      url,
    });
  });

  return bookmarks;
};
```

Here i have written a function to parse the bookmarks.

```tsx
const file = formData.get("bmrk") as File;

// const filecontent = fs.readFileSync(file, 'utf-8');

const filecontent = await file.text();

const parsedBookmark = parseBookmarks(filecontent);
```

Reading the file content, and parse it, and return the bookmarks.
