# Nexa BillSuite — GitHub + Supabase + Render Setup (සිංහල Guide)

## 1. මූලික architecture

- **GitHub** = system source code තබන repository. Database එක නොවේ.
- **Render** = website එක + secure Node.js backend එක host කරන සේවාව.
- **Supabase** = PostgreSQL database එක.
- **Browser** = data RAM එකට load කරලා UI එක පෙන්වන ස්ථානය. Business data localStorage/sessionStorage තුළ save කරන්නේ නැහැ.
- Login session එක පවත්වාගැනීමට පමණක් **HttpOnly session cookie** එකක් පාවිච්චි වෙනවා. ඒක business data එකක් නොවේ.

## 2. Supabase වල tables හදන ආකාරය

1. https://supabase.com/dashboard වෙත යන්න.
2. `bwjwltmwxovpzorcjrmi` project එක තෝරන්න.
3. **SQL Editor** > **New query**.
4. ZIP file එකේ `db/schema.sql` file එකේ සම්පූර්ණ SQL එක paste කරන්න.
5. **Run** ක්ලික් කරන්න.
6. **Table Editor** හරහා `account`, `product`, `sale`, `sale_line`, `purchase`, `purchase_line`, `app_session` වැනි tables ඇති බව verify කරන්න.
7. **Connect** > **Session pooler** හරහා database connection URI copy කරන්න. Database password සමඟ එන URI එක GitHub/chat එකට paste කරන්න එපා.

## 3. Windows PC එකෙන් test කිරීම

1. Node.js 22 හෝ අලුත් version එකක් install කරන්න.
2. ZIP එක extract කරලා PowerShell එකෙන් ඒ folder එකට යන්න.
3. `Copy-Item .env.example .env` command එක run කරන්න.
4. `.env` file එක edit කරලා actual `DATABASE_URL` එක paste කරන්න. `APP_ORIGIN=http://localhost:3000` ලෙස තබන්න.
5. Initial account details `.env` එකට **තාවකාලිකව** set කරන්න. Username `AHW123@` ය. Initial password එක ලෙස ඔයා කලින් ලබාදුන් value එක යොදාගන්න.
6. `npm install` > `npm run seed:admin` > `npm start` run කරන්න.
7. Browser එකේ http://localhost:3000 open කරලා login වෙන්න.
8. පළමු login එකේ අලුත්, ශක්තිමත් password එකක් set කරන්න.
9. `.env` file එකෙන් `INITIAL_ADMIN_PASSWORD` line එක ඉවත් කරන්න.

## 4. GitHub upload කිරීම

1. GitHub වල new **Private repository** එකක් හදන්න.
2. `README.md` හි ඇති git commands භාවිතා කරන්න.
3. Upload කිරීමට පෙර `.env` file එක, database credentials, Supabase secret key, පැරණි HTML file එක අහම්බෙන් repository එකට add වෙලා නැති බව verify කරන්න.

## 5. Render deployment

1. https://dashboard.render.com/ > **New Web Service**.
2. GitHub repository එක connect කරන්න.
3. Build `npm install`; Start `npm start`.
4. Environment variables: `DATABASE_URL`, `NODE_ENV=production`, `APP_ORIGIN=https://YOUR-DOMAIN.onrender.com`.
5. Deploy වුණාට පසුව `/api/health` URL එක check කරන්න.
6. Login, items create, test GRN, sale, duplicate prevention හා second-device refresh verify කරන්න.

## 6. අනිවාර්යයෙන් අවධානය යොමු කරන්න

මේ project එක **Phase 1 — Cloud Foundation** එකයි. Existing original HTML එකේ screen design එක සම්පූර්ණයෙන් port කරලා නැහැ. Customer data/old local browser database එක auto migrate වෙන්නේ නැහැ. පරණ HTML එක public host කරන්න එපා. මුලින් backup export කරලා, production release එකට කලින් account permissions, batch costing, tax, returns, reports, barcode/thermal printing, Excel import හා දත්ත reconciliation complete කරන්න.
