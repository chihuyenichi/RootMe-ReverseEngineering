because this file is 
(base) root@chihuyenich:/mnt/c/daohuyenchi_server/CTF_downloads/rootme/reverse/PE-DotNet-Basic-Anti-Debug# file ch39.exe
ch39.exe: PE32 executable (GUI) Intel 80386 Mono/.Net assembly, for MS Windows, 2 sections

so we use dnSpy to decompile it 

in PE_Crackme, we will saw some function below 

```
public static string mqsldfksdfgljk(string data, string key)
		{
			int length = data.Length;
			int length2 = key.Length;
			checked
			{
				char[] array = new char[length - 1 + 1];
				int num = length - 1;
				for (int i = 0; i <= num; i++)
				{
					array[i] = Strings.ChrW(Strings.Asc(data[i]) ^ Strings.Asc(key[i % length2]));
				}
				return new string(array);
			}
		}

		// Token: 0x06000015 RID: 21 RVA: 0x00002440 File Offset: 0x00000640
		private void Button1_Click(object sender, EventArgs e)
		{
			byte[] bytes = Convert.FromBase64String("B29mVBJ7cDdtRAYAHh0GKw1yX1g4IV9QOA==");
			string @string = Encoding.UTF8.GetString(bytes);
			string key = "I_Gu3$$_Y0u_Ju5t_Fl4gg3d_!!!";
			string text = Form1.mqsldfksdfgljk(@string, key);
			bool flag = Operators.CompareString(this.TextBox1.Text, Form1.mqsldfksdfgljk(@string, key), false) == 0;
			if (flag)
			{
				Interaction.MsgBox("GG !!! Vous pouvez valider le challenge avec ce mot de passe.", MsgBoxStyle.OkOnly, null);
			}
			else
			{
				Interaction.MsgBox("Nop", MsgBoxStyle.OkOnly, null);
			}
		}

		// Token: 0x06000016 RID: 22 RVA: 0x000024B4 File Offset: 0x000006B4
		private void Form1_Load(object sender, EventArgs e)
		{
			try
			{
				Process[] processes = Process.GetProcesses();
				foreach (Process process in processes)
				{
					bool flag = Operators.CompareString("dnSpy", process.ProcessName, false) == 0;
					if (flag)
					{
						Interaction.MsgBox("dnSpy detected", MsgBoxStyle.OkOnly, null);
						base.Close();
					}
					else
					{
						bool flag2 = Operators.CompareString("ILSpy", process.ProcessName, false) == 0;
						if (flag2)
						{
							Interaction.MsgBox("ILSpy detected", MsgBoxStyle.OkOnly, null);
							Interaction.MsgBox(processes, MsgBoxStyle.OkOnly, null);
							base.Close();
						}
					}
				}
			}
			catch (Exception ex)
			{
				Interaction.MsgBox(ex.Message, MsgBoxStyle.OkOnly, null);
			}
		}

```

so we can write code for finding flag 
it's "exploit.py" in same folder 
