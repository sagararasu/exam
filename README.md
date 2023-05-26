package kiara.assses.com;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collection;
import java.util.List;
import java.util.stream.Collectors;

public class Student{
    private List<Student> students;

    public Student() {
        this.students = loadStudentDetails(); // Load student details on initialization
    }

    public List<Student> getStudentDetails(int pageNumber, int pageSize) {
        int startIndex = (pageNumber - 1) * pageSize;
        int endIndex = Math.min(startIndex + pageSize, students.size());
        if (startIndex >= endIndex) {
            return new ArrayList<>(); // Return an empty list if the page is out of range
        }
        return students.subList(startIndex, endIndex);
    }

    public List<Student> filterStudents(FilterCriteria criteria) {
        return students.stream()
                .filter(student -> matchesFilterCriteria(student, criteria))
                .collect(Collectors.toList());
    }

    private boolean matchesFilterCriteria(Student student, FilterCriteria criteria) {
        if (criteria.getId() != null && !student.getId().equals(criteria.getId())) {
            return false;
        }
        if (criteria.getName() != null && !student.getName().contains(criteria.getName())) {
            return false;
        }
        // Add more filtering conditions for other columns as needed
        return true;
    }

    private List<Student> getName() {
		// TODO Auto-generated method stub
		return null;
	}

	private Object getId() {
		// TODO Auto-generated method stub
		return null;
	}

	private List<Student> loadStudentDetails() {
        List<Student> students = new ArrayList<>();
        try (BufferedReader reader = new BufferedReader(new FileReader("student_data.csv"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                String[] parts = line.split(",");
                String id = parts[0];
                String name = parts[1];
                int totalMarks = Integer.parseInt(parts[2]);
                students.addAll((Collection<? extends Student>) new Student1(id, name, totalMarks));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        return students;
    }
}

class Student1 {
    private String id;
    private String name;
    private int totalMarks;

    public Student1(String id, String name, int totalMarks) {
        this.id = id;
        this.name = name;
        this.totalMarks = totalMarks;
    }

    // Getters and setters

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getTotalMarks() {
        return totalMarks;
    }

    public void setTotalMarks(int totalMarks) {
        this.totalMarks = totalMarks;
    }
}

class FilterCriteria {
    private String id;
    private String name;

    // Getters and setters

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
