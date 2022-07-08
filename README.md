# Hibernate-Project-2 @OneToOne
package com.dto.jsp;

import java.util.List;
import java.util.Scanner;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;
import javax.persistence.Query;


public class All_InOne {

	static EntityManagerFactory f = Persistence.createEntityManagerFactory("shivanand");
	static EntityManager m = f.createEntityManager();
	static EntityTransaction t = m.getTransaction();
	public static Hospital saveData(Hospital h, Adress a){
		t.begin();
		m.persist(h);
		m.persist(a);
		t.commit();
		System.out.println("Data Saved");
		System.out.println("---------------------------------------");
		return h;
	}
	public static Hospital updateData(Hospital h, Adress a){
		t.begin();
		m.merge(h);
		m.merge(a);
		t.commit();
		System.out.println("Data Updated");
		System.out.println("---------------------------------------");
		return h;

	}
	public static Boolean deleteData(Hospital h, Adress a){
		Hospital z = m.find(Hospital.class, h.getId());
		Adress y = m.find(Adress.class, a.getId());
		if(z!=null && y!=null){
			t.begin();
			m.remove(z);
			m.remove(y);
			t.commit();
			System.out.println("Data deleted");
			System.out.println("---------------------------------------");
			return true;
		}
		else
			return false;
	}
	public static Hospital getData(Hospital h){
		Query q = m.createQuery("select h from Hospital h where h.id=?1");		
		q.setParameter(1, h.getId());
		List<Hospital> l = q.getResultList();
		if(l.size()>0){
			return l.get(0);
		}
		else
			return null;

	}
	public static Adress getData(Adress a){
		Query q2 =m.createQuery("select a from Adress a where a.id=?1");
		q2.setParameter(1, a.getId());
		List<Adress> l = q2.getResultList();

		if(l.size()>0){
			return l.get(0);
		}
		else
			return null;

	}
	public static List<Hospital> getAll(Hospital h){
		Query q= m.createQuery("select h from Hospital h");
		List<Hospital> l = q.getResultList();
		if(l!=null){
			return l;
		}
		else
			return null;


	}
	public static List<Adress> getAll(Adress a){
		Query q= m.createQuery("select a from Adress a");
		List<Adress> l = q.getResultList();
		if(l!=null){
			return l;
		}
		else
			return null;


	}

	public static void main(String[] args) {
		Scanner s = new Scanner(System.in);
	while(true){
		System.out.println("-------------------------------------------------------------");
		System.out.println("1:Add\n2:Update\n3:Delete\n4:Get One Data\n5:GetAll\n6:Exit");
		System.out.println("Enter your choice ");
		System.out.println("-------------------------------------------------------------");

		Hospital h = new Hospital();
		Adress a = new Adress();
		switch(s.nextInt()){
		case 1:
			System.out.println("Enter Name, founder, GST");
			h.setName(s.next());
			h.setFounder(s.next());
			h.setGst(s.nextDouble());
			System.out.println("Enter City, State, COuntry, Pincode ");
			a.setCity(s.next());
			a.setState(s.next());
			a.setCountry(s.next());
			a.setPincode(s.nextInt());
			h.setA(a);
			saveData(h,a);
			break;
		case 2:
			System.out.println("ENter id you want to Update");
			int id = s.nextInt();
			h.setId(id);
			a.setId(id);
			System.out.println("Enter Name, founder, GST");
			h.setName(s.next());
			h.setFounder(s.next());
			h.setGst(s.nextDouble());
			System.out.println("Enter City, State, COuntry, Pincode ");
			a.setCity(s.next());
			a.setState(s.next());
			a.setCountry(s.next());
			a.setPincode(s.nextInt());
			h.setA(a);
			updateData(h,a);
			break;
		case 3:
			System.out.println("Enter the id you want to delete");
			int v = s.nextInt();
			h.setId(v);
			a.setId(v);
			boolean g = deleteData(h, a);
			if(g)
				System.out.println("Data deleted");
			else
				System.out.println("Invalid Id");
			break;
		case 4:
			System.out.println("ENter the id you want to display");
			int id3=s.nextInt();
			a.setId(id3);
			h.setId(id3);
			Hospital e = getData(h);
			Adress a1 = getData(a);
			if(e!=null && a1!=null){
				System.out.println("-------------------------------------------------------------");
				System.out.println(e.getId()+" "+e.getName()+" "+e.getFounder()+" "+e.getGst());
				System.out.println(a1.getCity()+" "+a1.getState()+" "+a1.getCountry()+" "+a1.getPincode());
				System.out.println("-------------------------------------------------------------");

			}
			else
				System.out.println("Data Doesnt exist");
			break;
		case 5:
			List<Hospital> w = getAll(h);
			List<Adress> w2 = getAll(a);
			for(Hospital i : w){
				System.out.println(i.getId()+" "+i.getName()+" "+i.getFounder()+" "+i.getGst());
				System.out.println("-------------------------------------------------------------");
			}
			for(Adress i : w2){
				System.out.println(i.getCity()+" "+i.getState()+" "+i.getCountry()+" "+i.getPincode());
				System.out.println("-------------------------------------------------------------");
			}
			break;
		case 6:
			System.out.println("Thank you");
			System.out.println("-------------------------------------------------------------");

			System.exit(0);
		default :
			System.out.println("Invalid Choice");
			System.out.println("-------------------------------------------------------------");

		}
	}
	}
}
