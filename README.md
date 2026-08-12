<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>🎓 Haramaya University - Student Manager</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .student-card {
            transition: all 0.3s ease;
        }
        .student-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
        }
        .fade-in {
            animation: fadeIn 0.4s ease;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .badge {
            transition: all 0.2s ease;
        }
        .badge:hover {
            transform: scale(1.05);
        }
        input:focus, select:focus {
            outline: none;
            ring: 2px solid #4f46e5;
        }
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: #4f46e5;
            border-radius: 10px;
        }
        .empty-state {
            background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
            border: 2px dashed #94a3b8;
        }
        .grade-A { background: #d1fae5; color: #065f46; }
        .grade-B { background: #dbeafe; color: #1e40af; }
        .grade-C { background: #fef3c7; color: #92400e; }
        .grade-D { background: #fed7aa; color: #9a3412; }
        .grade-F { background: #fee2e2; color: #991b1b; }
        .university-header {
            background: linear-gradient(135deg, #1e3a5f 0%, #2d5a87 100%);
        }
        .gold-accent {
            color: #f5c842;
        }
        .course-card {
            transition: all 0.3s ease;
        }
        .course-card:hover {
            transform: scale(1.02);
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body>
    <div id="root"></div>

    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <script type="text/babel">
        const { useState, useMemo } = React;

        // ============================================================
        // 1. COURSE LIST (From the image)
        // ============================================================
        const COURSES = [
            'Communicative English Language Skills II',
            'Social Anthropology',
            'History of Ethiopia and the Horn',
            'Introduction to Emerging Technologies',
            'Computer Programming',
            'Applied Mathematics I',
            'Moral and Civic Education'
        ];

        // ============================================================
        // 2. STUDENT DATA (From the images + Andualem Guchi)
        // ============================================================
        const INITIAL_STUDENTS = [
            // From first image
            { id: 1, name: 'Dilbi Jemal Ahmed', department: 'Computer Science', year: 1 },
            { id: 2, name: 'Dubay Hussen Adem', department: 'Computer Science', year: 1 },
            { id: 3, name: 'Dugasa Mohammed Umer DR', department: 'Computer Science', year: 1 },
            { id: 4, name: 'Ebisa Juhar Mohammed', department: 'Computer Science', year: 1 },
            { id: 5, name: 'Ebisa Mohammed Seido DR', department: 'Computer Science', year: 1 },
            { id: 6, name: 'Eden Getachew Eshete', department: 'Computer Science', year: 1 },
            { id: 7, name: 'Edilawit Getachew Bogale', department: 'Computer Science', year: 1 },
            { id: 8, name: 'Edilawit Habtamu Wondifraw', department: 'Computer Science', year: 1 },
            
            // From second image
            { id: 9, name: 'Anisa Mohammed Musa', department: 'Computer Science', year: 1 },
            { id: 10, name: 'Bedatu Kelifa Hashim', department: 'Computer Science', year: 1 },
            { id: 11, name: 'Bedatu Mohammed Aliyi', department: 'Computer Science', year: 1 },
            { id: 12, name: 'Bekelcha Mohammed Sani Hashimo', department: 'Computer Science', year: 1 },
            { id: 13, name: 'Birki Mohammed Jemal', department: 'Computer Science', year: 1 },
            { id: 14, name: 'Biskute Awlachew Girma', department: 'Computer Science', year: 1 },
            { id: 15, name: 'Bona Ziyado Adem', department: 'Computer Science', year: 1 },
            { id: 16, name: 'Chala Abdula Mohammed', department: 'Computer Science', year: 1 },
            { id: 17, name: 'Chala Nure Sufiyan NC', department: 'Computer Science', year: 1 },
            
            // Andualem Guchi
            { id: 18, name: 'Andualem Guchi', department: 'Computer Science', year: 1 }
        ];

        // ============================================================
        // 3. GENERATE GRADES FOR STUDENTS
        // ============================================================
        const generateGrades = () => {
            const gradeLetters = ['A+', 'A', 'A-', 'B+', 'B', 'B-', 'C+', 'C', 'C-', 'D', 'F'];
            const gradeWeights = [5, 15, 12, 15, 18, 12, 10, 8, 5, 3, 2];
            let totalWeight = gradeWeights.reduce((a, b) => a + b, 0);
            let random = Math.random() * totalWeight;
            let cumulative = 0;
            for (let i = 0; i < gradeWeights.length; i++) {
                cumulative += gradeWeights[i];
                if (random <= cumulative) {
                    return gradeLetters[i];
                }
            }
            return 'B';
        };

        // Generate grades for each student
        const generateStudentGrades = (studentId) => {
            const grades = {};
            
            // Andualem Guchi (id: 18) - mostly A and A+, only 2 A-
            if (studentId === 18) {
                const andualemGrades = ['A', 'A+', 'A', 'A+', 'A', 'A-', 'A-'];
                COURSES.forEach((course, index) => {
                    grades[course] = andualemGrades[index];
                });
                return grades;
            }
            
            // For other students - random grades
            COURSES.forEach(course => {
                grades[course] = generateGrades();
            });
            return grades;
        };

        // Initialize students with grades
        const INITIAL_STUDENTS_WITH_GRADES = INITIAL_STUDENTS.map(student => ({
            ...student,
            grades: generateStudentGrades(student.id)
        }));

        // ============================================================
        // 4. MAIN APP COMPONENT
        // ============================================================
        function StudentManager() {
            // ---------- STATE ----------
            const [students, setStudents] = useState(INITIAL_STUDENTS_WITH_GRADES);
            const [searchTerm, setSearchTerm] = useState('');
            const [selectedStudent, setSelectedStudent] = useState(null);
            const [viewMode, setViewMode] = useState('grid');
            
            // Form state
            const [showForm, setShowForm] = useState(false);
            const [editingStudent, setEditingStudent] = useState(null);
            const [formData, setFormData] = useState({
                name: '',
                department: 'Computer Science',
                year: 1
            });

            // Notification state
            const [notification, setNotification] = useState({ message: '', type: '', visible: false });

            // ---------- DERIVED STATE ----------
            const filteredStudents = useMemo(() => {
                let result = [...students];
                if (searchTerm.trim()) {
                    const term = searchTerm.toLowerCase().trim();
                    result = result.filter(s =>
                        s.name.toLowerCase().includes(term)
                    );
                }
                return result;
            }, [students, searchTerm]);

            // ---------- HANDLERS ----------
            const showNotification = (message, type = 'success') => {
                setNotification({ message, type, visible: true });
                setTimeout(() => {
                    setNotification({ message: '', type: '', visible: false });
                }, 3000);
            };

            const addStudent = (newStudent) => {
                const student = {
                    ...newStudent,
                    id: Date.now(),
                    grades: generateStudentGrades(Date.now())
                };
                setStudents(prev => [...prev, student]);
                showNotification(`✅ ${student.name} added successfully!`, 'success');
            };

            const updateStudent = (id, updatedData) => {
                setStudents(prev =>
                    prev.map(s =>
                        s.id === id
                            ? { ...s, ...updatedData }
                            : s
                    )
                );
                showNotification(`✏️ Student updated successfully!`, 'info');
            };

            const deleteStudent = (id) => {
                if (window.confirm('Are you sure you want to delete this student?')) {
                    const student = students.find(s => s.id === id);
                    setStudents(prev => prev.filter(s => s.id !== id));
                    if (selectedStudent?.id === id) setSelectedStudent(null);
                    showNotification(`🗑️ ${student?.name || 'Student'} removed!`, 'error');
                }
            };

            const resetForm = () => {
                setFormData({ name: '', department: 'Computer Science', year: 1 });
                setEditingStudent(null);
                setShowForm(false);
            };

            const handleSubmit = (e) => {
                e.preventDefault();
                if (!formData.name.trim()) {
                    showNotification('⚠️ Please enter student name!', 'error');
                    return;
                }
                if (editingStudent) {
                    updateStudent(editingStudent.id, formData);
                } else {
                    addStudent(formData);
                }
                resetForm();
            };

            const handleEdit = (student) => {
                setEditingStudent(student);
                setFormData({
                    name: student.name,
                    department: student.department,
                    year: student.year
                });
                setShowForm(true);
                setSelectedStudent(null);
                window.scrollTo({ top: 0, behavior: 'smooth' });
            };

            // ---------- STATS ----------
            const stats = useMemo(() => {
                const total = students.length;
                return { total };
            }, [students]);

            // Calculate GPA for a student
            const calculateGPA = (grades) => {
                const gradePoints = {
                    'A+': 4.0, 'A': 4.0, 'A-': 3.7, 'B+': 3.3, 'B': 3.0,
                    'B-': 2.7, 'C+': 2.3, 'C': 2.0, 'C-': 1.7,
                    'D': 1.0, 'F': 0.0
                };
                let totalPoints = 0;
                let count = 0;
                Object.values(grades).forEach(grade => {
                    if (gradePoints[grade] !== undefined) {
                        totalPoints += gradePoints[grade];
                        count++;
                    }
                });
                return count > 0 ? (totalPoints / count).toFixed(2) : '0.00';
            };

            // ---------- RENDER ----------
            return (
                <div className="min-h-screen bg-gray-50">
                    {/* Notification Toast */}
                    {notification.visible && (
                        <div className={`fixed top-4 right-4 z-50 p-4 rounded-lg shadow-lg fade-in max-w-md 
                            ${notification.type === 'success' ? 'bg-green-500' :
                              notification.type === 'error' ? 'bg-red-500' :
                              notification.type === 'info' ? 'bg-blue-500' : 'bg-gray-700'} text-white`}>
                            {notification.message}
                        </div>
                    )}

                    {/* University Header */}
                    <div className="university-header text-white py-6 px-4 md:px-8 shadow-lg">
                        <div className="max-w-7xl mx-auto">
                            <div className="flex flex-col md:flex-row justify-between items-start md:items-center">
                                <div>
                                    <h1 className="text-3xl md:text-4xl font-bold flex items-center gap-3">
                                        <span>🏛️</span>
                                        <span>Haramaya University</span>
                                    </h1>
                                    <p className="text-blue-200 mt-1 text-sm md:text-base">
                                        College of Computing and Informatics • Department of Computer Science
                                    </p>
                                </div>
                                <div className="flex items-center gap-4 mt-3 md:mt-0">
                                    <span className="bg-white/20 px-4 py-2 rounded-lg text-sm font-medium">
                                        🎓 {stats.total} Students
                                    </span>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div className="max-w-7xl mx-auto p-4 md:p-8">
                        {/* Action Bar */}
                        <div className="flex flex-col md:flex-row justify-between items-start md:items-center gap-4 mb-6">
                            <button
                                onClick={() => {
                                    resetForm();
                                    setShowForm(!showForm);
                                    setSelectedStudent(null);
                                }}
                                className={`px-6 py-3 rounded-lg font-semibold transition-all shadow-md flex items-center gap-2
                                    ${showForm
                                        ? 'bg-gray-500 hover:bg-gray-600 text-white'
                                        : 'bg-indigo-600 hover:bg-indigo-700 text-white hover:shadow-lg'
                                    }`}
                            >
                                {showForm ? '✕ Close Form' : '➕ Add Student'}
                            </button>
                            <div className="flex gap-2">
                                <button
                                    onClick={() => setViewMode('grid')}
                                    className={`px-4 py-2 rounded-lg transition ${viewMode === 'grid'
                                        ? 'bg-indigo-600 text-white'
                                        : 'bg-white text-gray-600 hover:bg-gray-100'
                                    }`}
                                >
                                    📐 Grid
                                </button>
                                <button
                                    onClick={() => setViewMode('list')}
                                    className={`px-4 py-2 rounded-lg transition ${viewMode === 'list'
                                        ? 'bg-indigo-600 text-white'
                                        : 'bg-white text-gray-600 hover:bg-gray-100'
                                    }`}
                                >
                                    📋 List
                                </button>
                            </div>
                        </div>

                        {/* Add/Edit Form */}
                        {showForm && (
                            <div className="bg-white rounded-xl shadow-lg p-6 mb-8 fade-in border border-gray-100">
                                <h2 className="text-xl font-bold text-gray-800 mb-4">
                                    {editingStudent ? '✏️ Edit Student' : '➕ Add New Student'}
                                </h2>
                                <form onSubmit={handleSubmit} className="grid grid-cols-1 md:grid-cols-3 gap-4">
                                    <div>
                                        <label className="block text-sm font-medium text-gray-700 mb-1">Full Name *</label>
                                        <input
                                            type="text"
                                            value={formData.name}
                                            onChange={e => setFormData({ ...formData, name: e.target.value })}
                                            className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-400"
                                            placeholder="John Doe"
                                            required
                                        />
                                    </div>
                                    <div>
                                        <label className="block text-sm font-medium text-gray-700 mb-1">Department</label>
                                        <input
                                            type="text"
                                            value={formData.department}
                                            onChange={e => setFormData({ ...formData, department: e.target.value })}
                                            className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-400"
                                            placeholder="Computer Science"
                                        />
                                    </div>
                                    <div>
                                        <label className="block text-sm font-medium text-gray-700 mb-1">Year</label>
                                        <input
                                            type="number"
                                            value={formData.year}
                                            onChange={e => setFormData({ ...formData, year: parseInt(e.target.value) || 1 })}
                                            className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-400"
                                            min="1"
                                            max="5"
                                        />
                                    </div>
                                    <div className="md:col-span-3 flex gap-3 pt-2">
                                        <button
                                            type="submit"
                                            className="px-6 py-2 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 transition shadow-md"
                                        >
                                            {editingStudent ? '💾 Update Student' : '➕ Add Student'}
                                        </button>
                                        <button
                                            type="button"
                                            onClick={resetForm}
                                            className="px-6 py-2 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300 transition"
                                        >
                                            Cancel
                                        </button>
                                    </div>
                                </form>
                            </div>
                        )}

                        {/* Search Bar */}
                        <div className="bg-white rounded-xl shadow p-4 mb-6">
                            <div className="relative">
                                <span className="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400">🔍</span>
                                <input
                                    type="text"
                                    placeholder="Search students by name..."
                                    value={searchTerm}
                                    onChange={e => setSearchTerm(e.target.value)}
                                    className="w-full pl-10 pr-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-400 text-lg"
                                />
                            </div>
                        </div>

                        {/* Student List */}
                        {filteredStudents.length === 0 ? (
                            <div className="empty-state rounded-xl p-12 text-center">
                                <p className="text-5xl mb-4">📚</p>
                                <h3 className="text-xl font-semibold text-gray-700">No Students Found</h3>
                                <p className="text-gray-500 mt-2">
                                    {students.length === 0
                                        ? 'Start by adding your first student!'
                                        : 'Try adjusting your search.'}
                                </p>
                            </div>
                        ) : viewMode === 'grid' ? (
                            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                                {filteredStudents.map(student => (
                                    <StudentCard
                                        key={student.id}
                                        student={student}
                                        onEdit={handleEdit}
                                        onDelete={deleteStudent}
                                        onView={setSelectedStudent}
                                        calculateGPA={calculateGPA}
                                    />
                                ))}
                            </div>
                        ) : (
                            <div className="bg-white rounded-xl shadow overflow-hidden">
                                <table className="w-full">
                                    <thead className="bg-gray-50 border-b">
                                        <tr>
                                            <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">#</th>
                                            <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Name</th>
                                            <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Department</th>
                                            <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Year</th>
                                            <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">GPA</th>
                                            <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Actions</th>
                                        </tr>
                                    </thead>
                                    <tbody className="divide-y divide-gray-200">
                                        {filteredStudents.map((student, index) => (
                                            <tr key={student.id} className="hover:bg-gray-50 transition">
                                                <td className="px-6 py-4">{index + 1}</td>
                                                <td className="px-6 py-4 font-medium">
                                                    {student.name}
                                                </td>
                                                <td className="px-6 py-4">{student.department}</td>
                                                <td className="px-6 py-4">Year {student.year}</td>
                                                <td className="px-6 py-4 font-bold text-indigo-600">
                                                    {calculateGPA(student.grades)}
                                                </td>
                                                <td className="px-6 py-4 flex gap-2">
                                                    <button
                                                        onClick={() => setSelectedStudent(student)}
                                                        className="px-3 py-1 bg-blue-50 text-blue-600 rounded hover:bg-blue-100 text-sm"
                                                    >
                                                        📊 View
                                                    </button>
                                                    <button
                                                        onClick={() => handleEdit(student)}
                                                        className="px-3 py-1 bg-indigo-50 text-indigo-600 rounded hover:bg-indigo-100 text-sm"
                                                    >
                                                        ✏️
                                                    </button>
                                                    <button
                                                        onClick={() => deleteStudent(student.id)}
                                                        className="px-3 py-1 bg-red-50 text-red-600 rounded hover:bg-red-100 text-sm"
                                                    >
                                                        🗑️
                                                    </button>
                                                </td>
                                            </tr>
                                        ))}
                                    </tbody>
                                </table>
                            </div>
                        )}

                        {/* Course List */}
                        <div className="mt-8 bg-white rounded-xl shadow p-6">
                            <h2 className="text-xl font-bold text-gray-800 mb-4 flex items-center gap-2">
                                📚 Department Courses
                            </h2>
                            <div className="flex flex-wrap gap-2">
                                {COURSES.map((course, index) => (
                                    <span
                                        key={index}
                                        className="course-card px-4 py-2 bg-indigo-50 text-indigo-700 rounded-full text-sm font-medium border border-indigo-200"
                                    >
                                        {course}
                                    </span>
                                ))}
                            </div>
                        </div>

                        {/* Student Detail Modal */}
                        {selectedStudent && (
                            <StudentDetailModal
                                student={selectedStudent}
                                onClose={() => setSelectedStudent(null)}
                                courses={COURSES}
                                calculateGPA={calculateGPA}
                            />
                        )}
                    </div>
                </div>
            );
        }

        // ============================================================
        // 5. STUDENT CARD COMPONENT
        // ============================================================
        function StudentCard({ student, onEdit, onDelete, onView, calculateGPA }) {
            const getInitials = (name) => {
                return name
                    .split(' ')
                    .map(word => word[0])
                    .join('')
                    .toUpperCase()
                    .slice(0, 2);
            };

            const gpa = calculateGPA(student.grades);
            const gpaColor = gpa >= 3.5 ? 'text-green-600' : gpa >= 3.0 ? 'text-blue-600' : gpa >= 2.0 ? 'text-yellow-600' : 'text-red-600';

            return (
                <div className="student-card bg-white rounded-xl shadow-md overflow-hidden border border-gray-100">
                    <div className="p-5">
                        <div className="flex items-start justify-between">
                            <div className="flex items-center gap-3">
                                <div className="w-12 h-12 rounded-full bg-indigo-100 text-indigo-700 flex items-center justify-center font-bold text-lg">
                                    {getInitials(student.name)}
                                </div>
                                <div>
                                    <h3 className="font-semibold text-gray-800 text-lg">
                                        {student.name}
                                    </h3>
                                    <p className="text-sm text-gray-500">{student.department} • Year {student.year}</p>
                                </div>
                            </div>
                            <span className={`badge px-3 py-1 rounded-full text-sm font-medium ${gpaColor} bg-opacity-10 bg-gray-100`}>
                                GPA: {gpa}
                            </span>
                        </div>

                        <div className="mt-4 flex gap-2">
                            <button
                                onClick={() => onView(student)}
                                className="flex-1 px-3 py-1.5 bg-blue-50 text-blue-600 rounded-lg hover:bg-blue-100 transition text-sm font-medium"
                            >
                                📊 View Grades
                            </button>
                            <button
                                onClick={() => onEdit(student)}
                                className="flex-1 px-3 py-1.5 bg-indigo-50 text-indigo-600 rounded-lg hover:bg-indigo-100 transition text-sm font-medium"
                            >
                                ✏️ Edit
                            </button>
                            <button
                                onClick={() => onDelete(student.id)}
                                className="flex-1 px-3 py-1.5 bg-red-50 text-red-600 rounded-lg hover:bg-red-100 transition text-sm font-medium"
                            >
                                🗑️
                            </button>
                        </div>
                    </div>
                </div>
            );
        }

        // ============================================================
        // 6. STUDENT DETAIL MODAL
        // ============================================================
        function StudentDetailModal({ student, onClose, courses, calculateGPA }) {
            const gradeColors = {
                'A+': 'bg-green-100 text-green-800',
                'A': 'bg-green-100 text-green-800',
                'A-': 'bg-green-100 text-green-700',
                'B+': 'bg-blue-100 text-blue-800',
                'B': 'bg-blue-100 text-blue-700',
                'B-': 'bg-blue-100 text-blue-600',
                'C+': 'bg-yellow-100 text-yellow-800',
                'C': 'bg-yellow-100 text-yellow-700',
                'C-': 'bg-yellow-100 text-yellow-600',
                'D': 'bg-orange-100 text-orange-800',
                'F': 'bg-red-100 text-red-800',
            };

            const gpa = calculateGPA(student.grades);

            return (
                <div className="fixed inset-0 bg-black/50 flex items-center justify-center z-50 p-4 fade-in" onClick={onClose}>
                    <div className="bg-white rounded-2xl shadow-2xl max-w-2xl w-full max-h-[90vh] overflow-y-auto" onClick={e => e.stopPropagation()}>
                        <div className="sticky top-0 bg-white border-b p-4 flex justify-between items-center">
                            <div>
                                <h2 className="text-2xl font-bold text-gray-800">
                                    {student.name}
                                </h2>
                                <p className="text-gray-500">{student.department} • Year {student.year}</p>
                            </div>
                            <button onClick={onClose} className="text-gray-400 hover:text-gray-600 text-2xl">
                                ✕
                            </button>
                        </div>

                        <div className="p-6">
                            <div className="flex justify-between items-center mb-6">
                                <h3 className="text-lg font-semibold text-gray-700">📊 Grade Report</h3>
                                <span className={`px-4 py-2 rounded-lg font-bold text-lg ${gpa >= 3.5 ? 'bg-green-100 text-green-700' :
                                        gpa >= 3.0 ? 'bg-blue-100 text-blue-700' :
                                        gpa >= 2.0 ? 'bg-yellow-100 text-yellow-700' :
                                        'bg-red-100 text-red-700'
                                    }`}>
                                    GPA: {gpa}
                                </span>
                            </div>

                            <div className="space-y-2">
                                {courses.map((course, index) => {
                                    const grade = student.grades[course] || 'N/A';
                                    const colorClass = gradeColors[grade] || 'bg-gray-100 text-gray-700';
                                    return (
                                        <div key={index} className="flex justify-between items-center p-3 bg-gray-50 rounded-lg hover:bg-gray-100 transition">
                                            <span className="font-medium text-gray-700">{course}</span>
                                            <span className={`px-4 py-1 rounded-full font-semibold ${colorClass}`}>
                                                {grade}
                                            </span>
                                        </div>
                                    );
                                })}
                            </div>

                            <div className="mt-6 pt-4 border-t">
                                <p className="text-sm text-gray-500">
                                    📍 Haramaya University • Department of Computer Science
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            );
        }

        // ============================================================
        // 7. RENDER APP
        // ============================================================
        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<StudentManager />);
    </script>
</body>
</html>
