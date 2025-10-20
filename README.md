# pythonweb


import streamlit as st  
from datetime import date  

st.set_page_config(page_title="login & profile", layout="centered")  

if "is_authed" not in st.session_state:  
    st.session_state.is_authed = False  
if "profile" not in st.session_state:  
    st.session_state.profile = {"name: ": "", "email: ": "", "dob": None}  
    
    
    
    
    st.title("Login & Profile Card")

# --- Login form
if not st.session_state.is_authed:
    with st.form("login"):
        u = st.text_input("Username")
        p = st.text_input("Password", type="password")
        ok = st.form_submit_button("Login")
    if ok:
        if len(u) >= 3 and len(p) >= 6:
            st.session_state.is_authed = True
            st.success("Login successful")
        else:
            st.error("Username must be >= 3 chars and password >= 6 chars.")
    st.stop()

# --- Edit profile form (only shown when logged in)
st.subheader("Edit profile")
with st.form("profile"):
    name = st.text_input("Full name", value=st.session_state.profile["name"])
    email = st.text_input("Email", value=st.session_state.profile["email"])
    dob = st.date_input(
        "Date of birth",
        value=st.session_state.profile["dob"]
    )
    submitted = st.form_submit_button("Save")
if submitted:
    if "@" not in email:
        st.warning("Enter a valid email address.")
    else:
        st.session_state.profile = {
            "name": name,
            "email": email,
            "dob": dob
        }
        st.success("Profile saved.")

# --- Profile card display
st.divider()
st.subheader("Profile Preview")
st.write("**Full name:**", st.session_state.profile["name"])
st.write("**Email:**", st.session_state.profile["email"])
st.write("**Date of Birth:**", st.session_state.profile["dob"])
