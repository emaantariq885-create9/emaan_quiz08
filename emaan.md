# CONNECTION WITH SUPABASE

## step 1--

First we create a nextjs project by using these commands

cd desktop
npx create-next-app@latest emaan-sup
cd emaan-sup
npm install @supabase/supabase-js
npm run dev


## step 2

Next create a supabase project in www.supabase.com
login with your gmail
create a project
now create a sql files for your note app schema

``sql
create table Users(
    user_id INT PRIMARY KEY,
    username VARCHAR(50) not null unique,
    email varchar(100)not null unique,
    password_hash varchar(255)not null,
    created_at timestamp default current_timestamp
);
create table categories(
 category_id int Primary,name varchar(30)not null unique
 description TEXT ,

)
create table tags
 tag_id int Primary key,name varchar(30)not null unique

  create table notes
  note_id int Primary key,user_id int not null,
  category_id int null, title varchar(150)not null,content
  text not null,is_archived boolean default
  false,is_deleted boolean default false,create_at timestamp default
  current_timestamp,updated_at timestamp default current_timestamp on
  update current_timestamp
  Foreign Key userid and categories_id  }
insert data into it
   select dataUntitled title

now run the files in sql editor

the tables in supabase will be created

Now use connect button to connect project with supabase 

## step3 


use supabase_url and supabase_publishable key in .env.local file like this

``javascript

NEXT_PUBLIC_SUPABASE_URL=https://anyprojectid.supabase.co
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=sb_publishable_key


now create lib folder in root folder and create supabase.js files
paste this all in it
`` javascript
import { createClient } from '@supabase/supabase-js';

export const supabase = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL,
  process.env.NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY  
);



import { revalidatePath } from 'next/cache';
import { supabase } from '@/lib/supabase';

async function addDepartment(formData) {
  'use server';
  const department_id = Number(formData.get('department_id'));
  const department_name = formData.get('department_name');
  const location = formData.get('location');

  const { error } = await supabase
    .from('department')
    .insert({ department_id, department_name, location });
  if (error) throw new Error(error.message);
  revalidatePath('/departments');
}
department/page.js

``javascript 
export default async function DepartmentsPage() {
  const { data: departments, error } = await supabase
    .from('department')
    .select('*')
    .order('department_id');

  return (
    <div>
      <h1>Departments</h1>
      

      <form action={addDepartment}>
        <label>Department ID</label>
        <input type="number" name="department_id" required />

        <label>Department Name</label>
        <input type="text" name="department_name" required />

        <label>Location</label>
        <input type="text" name="location" />

        <button type="submit">Add Department</button>
      </form>

      {error && <p className="error">Error: {error.message}</p>}

      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Location</th>
          </tr>
        </thead>
        <tbody>
          {departments?.map((d) => (
            <tr key={d.department_id}>
              <td>{d.department_id}</td>
              <td>{d.department_name}</td>
              <td>{   d.location}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

## step 4

test localhost:3000/department

if there is an error it will show here
