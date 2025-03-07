import streamlit as st
import os

# Set page config
st.set_page_config(page_title="My Digital Footprint", page_icon="🚀", layout="wide")

# Owner's Credentials (For Authentication)
OWNER_EMAIL = "uwiringiyimanajean01@gmail.com"
OWNER_PASSWORD = "uwiringiyimanajean"

# Initialize session state for authentication and login visibility
if "authenticated" not in st.session_state:
    st.session_state.authenticated = False
if "show_login" not in st.session_state:
    st.session_state.show_login = False

# Profile Details (Static)
PROFILE_DETAILS = {
    "name": "jeanne uwiringiyimana",
    "location": "Musanze, Rwanda",
    "field_of_study": "Software Engineering, Year 3",
    "university": "INES Ruhengeri",
    "about_me": """I am jeanne, an enthusiastic software engineering student set to graduate this year, driven by technology's potential to solve real-world challenges and enhance efficiency. 
    Currently, I am working on my final dissertation and developing a personal portfolio using Python Streamlit."""
}

# Sidebar Navigation
st.sidebar.title("📌 Navigation")
page = st.sidebar.radio("Go to", ["Home", "Projects", "Skills", "Testimonials", "Contact", "Timeline","Settings"])

# Authentication Section
if page == "Home":
    st.title("🚀 My Digital Footprint – Showcasing My Journey")

    # Button to show/hide login form
    if not st.session_state.authenticated:
        if st.button("🔐 Login to edit profile"):
            st.session_state.show_login = not st.session_state.show_login  # Toggle login form visibility

    # Display login form only if the button is clicked
    if st.session_state.show_login and not st.session_state.authenticated:
        with st.form("login_form"):
            st.subheader("🔑 Enter Admin Credentials")
            email = st.text_input("Email")
            password = st.text_input("Password", type="password")
            login_button = st.form_submit_button("Login")

            if login_button:
                if email == OWNER_EMAIL and password == OWNER_PASSWORD:
                    st.session_state.authenticated = True
                    st.success("✅ Login successful! You can now update your profile picture.")
                    st.session_state.show_login = False  # Hide form after login
                else:
                    st.error("❌ Invalid email or password. Access denied!")

    # Display Profile Section
    st.subheader("👩‍💻 About Me")

    # Profile Picture with Camera Icon Button
    col1, col2 = st.columns([1, 6])
    with col1:
        st.image("jeanne.jpg", width=150, caption="Profile Picture")  
    with col2:
        if st.session_state.authenticated:
            if st.button("📷", help="Update Profile Picture"):
                uploaded_image = st.file_uploader("Upload a new profile picture", type=["jpg", "png"])
                if uploaded_image:
                    with open("jeanne.jpg", "wb") as file:
                        file.write(uploaded_image.getbuffer())
                    st.success("✅ Profile picture updated successfully! Refresh to see changes.")

    # Display Profile Details
    st.write(f"👤 Name: {PROFILE_DETAILS['name']}")
    st.write(f"📍 Location: {PROFILE_DETAILS['location']}")
    st.write(f"📚 Field of Study: {PROFILE_DETAILS['field_of_study']}")
    st.write(f"🎓 University: {PROFILE_DETAILS['university']}")
    st.write(f"📝 About Me: {PROFILE_DETAILS['about_me']}")

    # About Me section
    st.subheader("About Me")
    about_me = st.text_area("Write a short description about yourself", PROFILE_DETAILS["about_me"])
    st.write(about_me)


# Projects Page with Filtering System
elif page == "Projects":
    st.title("💻 My Projects")

    project_filter = st.selectbox("Filter by Category", ["All", "Year 1", "Year 2", "Year 3", "Dissertation"])

    projects = {
        "Year 1": "Library Management System - Python & SQLite",
        "Year 2": "school Management System - Java & MySQL",
        "Year 3": "Car Rental System - Python & SQLite",
        "Dissertation": "AI-web based application for monitoring harvest health - Python & MySQL"
    }

    for year, description in projects.items():
        if project_filter == "All" or project_filter == year:
            with st.expander(f"📌 {year} - {description}"):
                st.write(f"*Project Type:* Individual")
                st.write(f"*Description:* A system designed for {description}.")
                st.write("🔗 [GitHub Repo](#)")

# Skills section
elif page == "Skills":
    st.title("⚡ Skills and Achievements")
    
    st.subheader("Programming Skills")
    skill_python = st.slider("Python", 0, 100, 90)
    st.progress(skill_python)
    
    skill_js = st.slider("JavaScript", 0, 100, 75)
    st.progress(skill_js)
    
    skill_AI = st.slider("Artificial Intelligence", 0, 100, 65)
    st.progress(skill_AI)

    skill_MachineLearning = st.slider("Machine Learning", 0, 100, 75)
    st.progress(skill_MachineLearning)

    skill_React = st.slider("React Js", 0, 100, 75)
    st.progress(skill_React)

    st.subheader("🏆 Certifications & Achievements")
    st.write("✔ AI in Research & Education")
    st.write("✔ Certified by NOVA SERVICES (Web application development, social media management, webpages creation, website administration and security).")
    st.write("✔ Certificate in TME EDUCATION AMBASSADOR IN RWANDA (Arduino, Python, and introduction to Robotics).")

# Testimonials Page
elif page == "Testimonials":
    st.title("🗣 Testimonials")
    st.write("🌟 She's a Motivator🤝 and Innovative Software Engineer! – Rachel")

    # Display existing testimonials
    st.subheader("💬 Share Your Testimonial")
    
    # User input for new testimonials
    new_testimonial = st.text_area("Write your testimonial here...")
    user_name = st.text_input("Your Name")

    if st.button("Submit"):
        if new_testimonial and user_name:
            st.success("Thank you for your testimonial!")
            st.write(f"🌟 {new_testimonial} – {user_name}")
        else:
            st.warning("Please enter both your testimonial and your name.")

# Timeline Page
elif page == "Timeline":
    st.title("⏳ My Academic & Project Milestones")

    timeline_data = [
        ("Year 1", "📚 First major project completed: Library Management System"),
        ("Year 2", "💻 Deep researches on different programming languages"),
        ("Year 3", "📖 Final Year Dissertation: AI-web based application for monitoring harvest health"),
    ]

    for year, event in timeline_data:
        st.write(f"**{year}:** {event}")

# Contact Page with Validation
elif page == "Contact":
    st.title("📬 Contact Me")

    with st.form("contact_form"):
        name = st.text_input("Your Name")
        email = st.text_input("Your Email")
        message = st.text_area("Your Message")
        submitted = st.form_submit_button("Send Message")

        if submitted:
            if name and email and message:
                st.success("✅ Message sent successfully!")
            else:
                st.warning("⚠ Please fill in all fields before submitting.")

    st.write("📧 Email: uwiringiyimanajean01@gmail.com")
    st.write("[🔗 LinkedIn](https://linkedin.com/in/uwiringiyimana-jeanne-775726354)")
    st.write("[📂 GitHub](https://github.com/ujeanne/student_portfolio.git)")
# Settings Page
elif page == "Settings":
    st.title("⚙️ Settings")
    # Theme Customization (Light/Dark Mode)
    theme = st.selectbox("Choose Theme", ["Light", "Dark"])
    if theme == "Dark":
        st.markdown("""
            <style>
                .main {background-color: #2E2E2E; color: white;}
                .stSidebar {background-color: #1E1E1E; color: white;}
                .stButton > button {background-color: #1F618D; color: white; border-radius: 10px;}
                .stExpander {background-color: #2E2E2E; color: white;}
                .stProgress {background-color: #D5D8DC;}
            </style>
        """, unsafe_allow_html=True)
    else:
        st.markdown("""
            <style>
                .main {background-color: #f5f7fa; color: black;}
                .stSidebar {background-color: #2E4053; color: white;}
                .stButton > button {background-color: #1F618D; color: white; border-radius: 10px;}
                .stExpander {background-color: #f5f7fa; color: black;}
                .stProgress {background-color: #D5D8DC;}
            </style>
        """, unsafe_allow_html=True)
    # Button to show/hide login form
    if not st.session_state.authenticated:
        if st.button("🔐 Login to Change Profile Details"):
            st.session_state.show_login = not st.session_state.show_login  # Toggle login form visibility

    # Display login form only if the button is clicked
    if st.session_state.show_login and not st.session_state.authenticated:
        with st.form("settings_login_form"):
            st.subheader("🔑 Admin Login Required")
            email = st.text_input("Email")
            password = st.text_input("Password", type="password")
            login_button = st.form_submit_button("Login")

            if login_button:
                if email == OWNER_EMAIL and password == OWNER_PASSWORD:
                    st.session_state.authenticated = True
                    st.success("✅ Login successful! You can now edit profile details.")
                    st.session_state.show_login = False  # Hide form after login
                else:
                    st.error("❌ Invalid email or password. Access denied!")

    # Show editable profile settings only if authenticated
    if st.session_state.authenticated:
        st.subheader("Edit Profile Details")
        name = st.text_input("Your Name", PROFILE_DETAILS["name"])
        location = st.text_input("Location", PROFILE_DETAILS["location"])
        field_of_study = st.text_input("Field of Study", PROFILE_DETAILS["field_of_study"])
        university = st.text_input("University", PROFILE_DETAILS["university"])
        about_me = st.text_area("About Me", PROFILE_DETAILS["about_me"])

        if st.button("Save Changes"):
            st.success("✅ Profile updated successfully!")

        # Profile Picture Upload
        st.subheader("🖼 Change Profile Picture")
        new_profile_picture = st.file_uploader("Upload a new profile picture", type=["jpg", "png"])

        if new_profile_picture:
            with open("jeanne.jpg", "wb") as file:
                file.write(new_profile_picture.getbuffer())
            st.success("✅ Profile picture updated successfully! Refresh to see changes.")
            st.image(new_profile_picture, width=150, caption="New Profile Picture")

