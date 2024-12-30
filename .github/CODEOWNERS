import streamlit as st
import requests

# Set your SerpApi API key
serpapi_api_key = "5e354959e3e052785f1abf5752f3893e479f6faa09bcc3e9150298e502f222a5"  # Replace with your actual API key

# Define a list of platforms for easy categorization
PLATFORMS = {
    'coursera': 'coursera.org',
    'udemy': 'udemy.com',
    'youtube': 'youtube.com',
    'edx': 'edx.org',
    'freecodecamp': 'freecodecamp.org',
    'futurelearn': 'futurelearn.com'
}


# Function to generate roadmap and resources using SerpApi
def generate_roadmap_and_resources(field_of_interest):
    query = f"top courses to learn {field_of_interest} from scratch"

    # Define SerpApi endpoint and parameters
    url = "https://serpapi.com/search"
    params = {
        "q": query,
        "api_key": serpapi_api_key,  # Your actual SerpApi API key
    }

    # Make the request to SerpApi
    response = requests.get(url, params=params)

    # Initialize lists to store courses by type
    free_courses = []
    certification_courses = []
    paid_courses = []
    certification_exams = []

    # Check if the response is successful
    if response.status_code == 200:
        # Get the response data in JSON format
        response_json = response.json()

        # Check if 'organic_results' is in the response
        if 'organic_results' in response_json:
            st.write("### Course Categories Based on Your Interest:")

            # Iterate through the organic search results and categorize them
            for result in response_json['organic_results']:
                course_title = result.get('title', 'No title available')
                course_link = result.get('link', '#')
                description = result.get('snippet', 'No description available')
                reviews = result.get('reviews', None)
                rating = result.get('rating', None)

                # Identify platform and categorize courses based on keywords
                platform = None
                for key, value in PLATFORMS.items():
                    if value in course_link.lower():
                        platform = key
                        break

                # If platform found, categorize
                if platform:
                    # Classify courses into free, certification, or paid
                    if 'free' in course_title.lower() or 'free' in description.lower():
                        free_courses.append((course_title, course_link, description, platform, rating, reviews))
                    elif 'certification' in course_title.lower() or 'certification' in description.lower():
                        certification_courses.append(
                            (course_title, course_link, description, platform, rating, reviews))
                        certification_exams.append((course_title, course_link))  # Separate list for certification exams
                    elif 'paid' in course_title.lower() or 'paid' in description.lower():
                        paid_courses.append((course_title, course_link, description, platform, rating, reviews))
                    else:
                        # Default to paid if nothing specific is found
                        paid_courses.append((course_title, course_link, description, platform, rating, reviews))

            # Sort courses by rating in descending order
            free_courses.sort(key=lambda x: x[4] if x[4] else 0, reverse=True)
            certification_courses.sort(key=lambda x: x[4] if x[4] else 0, reverse=True)
            paid_courses.sort(key=lambda x: x[4] if x[4] else 0, reverse=True)

            # Display sorted free courses
            if free_courses:
                st.write("### Top Free Courses")
                for course in free_courses:
                    with st.expander(course[0]):
                        st.markdown(f"**Platform**: {course[3].title()} - **Description**: {course[2]}")
                        st.markdown(f"**Rating**: {course[4] if course[4] else 'No rating available'}")
                        st.markdown(f"[Go to Course]({course[1]})")

            # Display sorted certification courses
            if certification_courses:
                st.write("### Top Certification Courses")
                for course in certification_courses:
                    with st.expander(course[0]):
                        st.markdown(f"**Platform**: {course[3].title()} - **Description**: {course[2]}")
                        st.markdown(f"**Rating**: {course[4] if course[4] else 'No rating available'}")
                        st.markdown(f"[Go to Course]({course[1]})")

            # Display sorted paid courses
            if paid_courses:
                st.write("### Top Paid Courses")
                for course in paid_courses:
                    with st.expander(course[0]):
                        st.markdown(f"**Platform**: {course[3].title()} - **Description**: {course[2]}")
                        st.markdown(f"**Rating**: {course[4] if course[4] else 'No rating available'}")
                        st.markdown(f"[Go to Course]({course[1]})")

            # Display Certification Exams
            if certification_exams:
                st.write("### Certification Exams")
                for exam in certification_exams:
                    with st.expander(exam[0]):
                        st.markdown(f"[Go to Certification Exam]({exam[1]})")

        else:
            st.write("No courses found. Please try another field or adjust your search.")
    else:
        st.error(f"Error fetching data from SerpApi: {response.status_code}")


# Function to get the survey responses
def get_survey_responses():
    # Ask for the field of interest (e.g., Data Science, Cyber Security, etc.)
    field_of_interest = st.text_input("Enter your field of interest (e.g., Data Science, Cyber Security, AI)")

    return field_of_interest


# Function to process survey responses and generate recommendations
def process_survey_responses(field_of_interest):
    if field_of_interest:
        st.write(f"Processing roadmap for field: {field_of_interest}")
        generate_roadmap_and_resources(field_of_interest)  # This will display the courses for the selected field


# Main function to run the app
def main():
    st.title("Career Roadmap Generator")

    # Get the survey responses and field of interest from the user
    field_of_interest = get_survey_responses()

    if field_of_interest:
        # Process the responses and generate recommendations and resources
        process_survey_responses(field_of_interest)


# Run the main function to start the Streamlit app
if __name__ == "__main__":
    main()
