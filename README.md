# HelloWorld





package com.qf.d_dianming;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.Scanner;

import com.qf.b_utils.CloseUtils;
import com.qf.c_login.DBUtils;

public class Test {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		clearCount();
		do{
			System.out.println("展示学生信息：");
			List<Student> list = getPersonList();
			for(Student s:list){
				System.out.print(s.getName()+"\t");
			}
			Random random = new Random();
			int index = random.nextInt(list.size());
			count(list.get(index).getName());
			System.out.println("\n抽到的同学是："+list.get(index).getName());
			System.out.println("是否继续(y/n)");
			if(!"y".equals(sc.next()))	break;
		}while(true);
		//展示学生信息及次数
		List<Student> list = getPersonList();
		for(Student s:list){
			System.out.println(s);
		}
	}
	private static void clearCount() {
		Connection cn = DBUtils.getConnection();
		String sql = "update t_stu set count=0";
		PreparedStatement prst = null;
		try {
			prst = cn.prepareStatement(sql);
			prst.executeUpdate();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}finally {
			CloseUtils.closeAll(prst,cn);
		}
	}
	private static List<Student> getPersonList() {
		List<Student> list = new ArrayList<>();
		Connection cn = DBUtils.getConnection();
		String sql = "Select name,count from t_stu";
		PreparedStatement prst = null;
		ResultSet rs = null;
		try {
			prst = cn.prepareStatement(sql);
			rs = prst.executeQuery();
			while(rs.next()){
				String name = rs.getString(1);
				int count = rs.getInt(2);
				Student s = new Student(name,count);
				list.add(s);
			}
			
		} catch (SQLException e) {
			e.printStackTrace();
		}finally {
			CloseUtils.closeAll(rs,prst,cn);
		}
		
		return list;
	}
	//count++
	private static Integer count(String name) {
		Connection cn = DBUtils.getConnection();
		String sql = "update t_stu set count=count+1 where name=?";
		PreparedStatement prst = null;
		int n = 0;
		try {
			prst = cn.prepareStatement(sql);
			prst.setString(1, name);
			n = prst.executeUpdate();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}finally {
			CloseUtils.closeAll(prst,cn);
		}
		return n;
	}




	private static List<String> getDataList() {
		List<String> list = new ArrayList<>();
		Connection cn = DBUtils.getConnection();
		String sql = "Select name from t_stu";
		PreparedStatement prst = null;
		ResultSet rs = null;
		try {
			prst = cn.prepareStatement(sql);
			rs = prst.executeQuery();
			while(rs.next()){
				list.add(rs.getString(1));
			}
			
		} catch (SQLException e) {
			e.printStackTrace();
		}finally {
			CloseUtils.closeAll(rs,prst,cn);
		}
		
		return list;
	}
}
