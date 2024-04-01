# Description: #
Can you try to get access to this website to get the flag?
You can download the source [here](https://artifacts.picoctf.net/c_atlas/34/app.tar.gz).

The website is running [here](http://atlas.picoctf.net:53436/). 

Can you log in?

# Solution: #
The website:
![img1](https://github.com/TTN-ATTN/assets/blob/13a4a47260f42b0fc5e40760bc9bcc55741c6170/pico2024/nosql/1.png)

After wodering around the source code for a while, I found a useful file.

seed.ts:
```ts
import User from "../models/user";


export const seedUsers = async (): Promise<void> => {
  
  try {

     const users = await User.find({email: "joshiriya355@mumbama.com"});
      if (users.length > 0) {
        return;
      }
    const newUser = new User({
      firstName: "Josh",
      lastName: "Iriya",
      email: "joshiriya355@mumbama.com",
      password: process.env.NEXT_PUBLIC_PASSWORD as string
    });
    await newUser.save();
  } catch (error) {
    throw new Error("Some thing went wrong")
  }
};
```

It seemed that I would have to login using the email joshiriya355@mumbama.com. So the problem boiled down to bypassing the password.

Next, I found the file for login processing, which is api\login\route.ts:

```ts
import User from "@/models/user";
import { connectToDB } from "@/utils/database";
import { seedUsers } from "@/utils/seed";

export const POST = async (req: any) => {
  const { email, password } = await req.json();
  try {
    await connectToDB();
    await seedUsers();
    const users = await User.find({
      email: email.startsWith("{") && email.endsWith("}") ? JSON.parse(email) : email,
      password: password.startsWith("{") && password.endsWith("}") ? JSON.parse(password) : password
    });

    if (users.length < 1)
      return new Response("Invalid email or password", { status: 401 });
    else {
      return new Response(JSON.stringify(users), { status: 200 });
    }
  } catch (error) {
    return new Response("Internal Server Error", { status: 500 });
  }
};
```
