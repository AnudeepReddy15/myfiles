using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.SqlClient;
using System.Data;

namespace ADO_NET
{
    class Program
    {
        static string conString = "Server=tcp:carpoolingvam.database.windows.net,1433;Initial Catalog=vamcarpooling;Persist Security Info=False;User ID=carpoolingvam;Password=NGTbatch2;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;";
        static SqlConnection con = new SqlConnection(conString);
        static SqlCommand cmd;
        static int newID;
        static void Main(string[] args)
        {
            getNewID();
            insertRecord();
            getData();
            //getDataUsingDataReader();
            //carpoolingvam.database.windows.net
        }

        private static void getNewID()
        {
            string cmdText = "Select max(ID)+1,456 from TestTable";
            cmd = new SqlCommand(cmdText, con);
            con.Open();
            newID=Convert.ToInt32(cmd.ExecuteScalar());
            con.Close();
        }

        private static void getDataUsingDataReader()
        {
            string cmdText = "Select * from TestTable";
            cmd = new SqlCommand(cmdText, con);
            con.Open();
            SqlDataReader dataReader = cmd.ExecuteReader();
            while (dataReader.Read())
            {
                Console.WriteLine("ID-" + dataReader["id"].ToString() + "..............Value-" + dataReader["test"].ToString());
            }
            con.Close();
            Console.ReadKey();
        }

        private static void getData()
        {
            //string id = "2";
            string cmdText = "Select * from TestTable";// where id=@id";
            //SqlParameter param = new SqlParameter("@id", id);
            cmd = new SqlCommand(cmdText, con);
            cmd.CommandType = CommandType.Text;
            //cmd.Parameters.Add(param);
            SqlDataAdapter da = new SqlDataAdapter(cmd);
            DataTable dt = new DataTable();
            da.Fill(dt);
            foreach (DataRow dr in dt.Rows)
            {
                Console.WriteLine("ID:" + dr["ID"].ToString() + "...Value:" + dr["Test"].ToString());
            }
            Console.ReadKey();
        }

        private static void insertRecord()
        {
            con.Open();
            cmd = new SqlCommand("insert into testTable values(" + newID.ToString() + ",'D')", con);
            int effectedRows = cmd.ExecuteNonQuery();
            Console.WriteLine("Number of rows inserted:" + effectedRows.ToString());
            con.Close();
        }
    }
}
